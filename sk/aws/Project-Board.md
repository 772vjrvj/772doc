## Environment variables (1)

| **Key**     | **Value**                                                    |
| ----------- | ------------------------------------------------------------ |
| MONGODB_URI | mongodb+srv://772vjrvj:0772@cluster0.wbhni.mongodb.net/blog?retryWrites=true&w=majority |
|             |                                                              |



```
const mongoose = require('mongoose');
const MONGODB_URI = process.env.MONGODB_URI;

/* Board 오브젝트를 정의합니다. */

const boardSchema = mongoose.Schema({
    id: {
        type: Number,
        require: true
    },
    name: {
        type: String,
        require: true
    },
    password: {
        type: String,
        require: true
    },
    content: {
        type: String,
        require: true
    },
    date: {
        type: Date,
        require: true
    }
    
})

/* 하나의 연결 객체를 반복적으로 상용합니다. */
let connection = null;

const connect = () => {
    if(connection && mongoose.connection.readyState === 1) {
        return Promise.resolve(connection);
    }
    else {
        return mongoose.connect(MONGODB_URI, { useNewUrlParser: true })
        .then(
            conn => {
                connection = conn;
                return connection;
            }
        );
    }
}



exports.handler = (event, context, callback) => {
    context.callbackWaitsForEmptyEventLoop = false;
    let operation = event.httpMethod;
    let Board = mongoose.model('board', boardSchema);
    let proxy, password;
    switch (operation) {
        case 'POST':
            /*
                경로: /board
                파라미터: {
                    "name": "작성자",
                    "content": "내용",
                    "password": "비밀번호"
                }
                설명: 특정 게시글을 작성합니다.
            */
            let lastId = 0;
            //가장 최근에 작성된 게시물 번호를 가져옵니다.
            connect().then(() =>
                Board.findOne({})
                    .sort({id: -1})
                    .exec(function(err, board) {
                        if(err) {
                            callback(null, {
                                'statusCode': 500,
                                'body': err
                            });
                        } else {
                            lastId = board ? board.id : 0;
                            const {name, content, password } =
                                JSON.parse(event.body);
                            const newBoard = new Board({
                                name, content, password
                            });
                            newBoard.date = new Date();
                            newBoard.id = lastId + 1;
                            //새로운 글을 등록합니다.
                            newBoard.save(function(err, board){
                                if(err) {
                                    callback(null, {
                                        'statusCode': 500,
                                        'body': err
                                    });
                                } else {
                                    callback(null, {
                                        'statusCode': 200,
                                        'body':JSON.stringify(lastId + 1)
                                    });
                                }
                            });
                        }
                    })
            );
            break;
        case 'GET':
            /*
                경로: /board
                설명: 전체 게시글 정보를 불러옵니다.
            */
            
            if(event.pathParameters === null) {
                let query = {};
                if(event.queryStringParameters !== null) {
                    if(event.queryStringParameters.name) {
                        query.name = {
                            $regex:event.queryStringParameters.name,
                            $option: 'i'
                        };
                    }
                    if(event.queryStringParameters.content) {
                        query.content = {
                            $regex:event.queryStringParameters.content,
                            $option: 'i'
                        };
                    }
                }
                // name 과 content를 이용하여 검색한 결과를 내림차순으로 반환합니다.
                connect().then(() => {
                    Board.find(query)
                        .select("-password")
                        .sort({id: -1})
                        .exec(function(err, boards) {
                            if(err) {
                                callback(null, {
                                    'statusCode': 500,
                                    'body': err
                                });
                            } else {
                                callback(null, {
                                    'statusCode': 200,
                                    'body':JSON.stringify(boards)
                                });
                            }
                        });
                });
            }
            /*
                경로: /board/:id
                설명: 특정 게시글 정보를 불러옵니다.
            */
            
            else{
                proxy = event.pathParameters.proxy;
                connect().then(() => {
                    Board.findOne({id:proxy})
                        .select("-password")
                        .exec(function(err, board) {
                            if(err) {
                                callback(null, {
                                    'statusCode': 500,
                                    'body': err
                                });
                            } else if(!board) {
                                callback(null, {
                                    'statusCode': 500,
                                    'body':JSON.stringify("Board not found")
                                });
                            } else {
                                callback(null, {
                                    'statusCode': 200,
                                    'body':JSON.stringify(board)
                                });
                            }
                        });
                    });
                }
            break;
        case 'PUT':
            /*
                경로: /board/:id
                헤더: password:"현재 비밀번호"
                파라미터: {
                    "name": "작성자",
                    "content": "내용",
                    "password": "비밀번호"
                }
                설명: 특정 게시글을 수정합니다.
            */
            proxy = event.pathParameters.proxy;
            password = event.headers.password;
            // 사용자가 입력한 번호의 게시물을 찾습니다.
            connect().then(() => {
                    Board.findOne({id:proxy})
                        .exec(function(err, board) {
                            if(err) {
                                callback(null, {
                                    'statusCode': 500,
                                    'body': err
                                });
                            } else if(!board) {
                                callback(null, {
                                    'statusCode': 500,
                                    'body':JSON.stringify("Board not found")
                                });
                            } else {
                                if(board.password !== password){
                                    callback(null, {
                                        'statusCode': 500,
                                        'body': JSON.stringify("Password is not correct.")
                                    });
                                } else {
                                    const {name, content, password } =
                                        JSON.parse(event.body);
                                        //사용자가 입력한 데이터에 맞게 정보를 변경합니다.
                                    Board.findOneAndUpdate(
                                        {id:proxy},
                                        {name, content, password}
                                    ).exec(function(err, board){
                                        if(err) {
                                            callback(null, {
                                                'statusCode': 500,
                                                'body': err
                                            });
                                        } else {
                                            callback(null, {
                                                'statusCode': 200,
                                                'body':JSON.stringify("Success")
                                            });
                                        }
                                    });
                                }
                            }
                        });
                    });
                    break;
                case "DELETE":
                    /*
                        경로: /board/:id
                        헤더: password:"현재 비밀번호"
                        설명: 특정 게시글을 삭제합니다.
                    */
                    proxy = event.pathParameters.proxy;
                    password = event.headers.password;
                            
                    connect().then(() => {
                            Board.findOne({id:proxy})
                                .exec(function(err, board) {
                                    if(err) {
                                        callback(null, {
                                            'statusCode': 500,
                                            'body': err
                                        });
                                    } else if(!board) {
                                        callback(null, {
                                            'statusCode': 500,
                                            'body':JSON.stringify("Board not found")
                                        });
                                    } else {
                                        if(board.password !== password){
                                            callback(null, {
                                                'statusCode': 500,
                                                'body': JSON.stringify("Password is not correct.")
                                            });
                                        } else {
                                                //사용자가 입력한 번호의 게시물을 삭제합니다.
                                            Board.findOneAndRemove(
                                                {id:proxy}
                                            ).exec(function(err, board){
                                                if(err) {
                                                    callback(null, {
                                                        'statusCode': 500,
                                                        'body': err
                                                    });
                                                } else {
                                                    callback(null, {
                                                        'statusCode': 200,
                                                        'body':JSON.stringify("Success")
                                                    });
                                                }
                                            });
                                        }
                                    }
                                });
                            });
                    break;
        default:
            callback(new Error(`Operation Error: "${operation}"`));
    }
};

```

