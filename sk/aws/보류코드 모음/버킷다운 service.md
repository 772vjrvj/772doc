```
package skt.mno.mpai.test.ext_interface.util.S3;

import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;
import software.amazon.awssdk.core.ResponseInputStream;
import software.amazon.awssdk.services.s3.S3Client;
import software.amazon.awssdk.services.s3.model.*;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.List;
import java.util.ListIterator;
import java.util.stream.Collectors;

@Slf4j
@Service
public class S3Downloader {
    private final S3Client s3Client;
    private final String domainRest = "s3.ap-northeast-2.amazonaws.com";

    @Value("${app.s3-bucket}")
    private String INTEREST_CATEGORY_META_BUCKET ;

    @Value("${app.interest-category.filepath}")
    private String INTEREST_CATEGORY_META_KEY  ;

    @Autowired
    public S3Downloader(S3Client s3Client) {
        this.s3Client = s3Client;
    }

    public String interestCategoryMeta(){
        log.info("interestCategoryMeta : bucket = {}, key = {}",INTEREST_CATEGORY_META_BUCKET,INTEREST_CATEGORY_META_KEY);

        ResponseInputStream<GetObjectResponse> resInputStream =  s3Client.getObject(requestFor(INTEREST_CATEGORY_META_BUCKET,INTEREST_CATEGORY_META_KEY));

        BufferedReader reader = new BufferedReader(new InputStreamReader(resInputStream));

        String fileContents = reader.lines().collect(Collectors.joining("\n"));

        return fileContents ;
    }

    public ResponseInputStream<GetObjectResponse> interestCategoryMeta(String key){
        log.info("interestCategoryMeta : bucket = {}, key = {}",INTEREST_CATEGORY_META_BUCKET,key);

        ResponseInputStream<GetObjectResponse> resInputStream =  s3Client.getObject(requestFor(INTEREST_CATEGORY_META_BUCKET,key));


        return resInputStream;
    }


    public List<S3Object> listBucketObjects() {

        try {
            ListObjectsRequest listObjects = ListObjectsRequest
                    .builder()
                    .bucket(INTEREST_CATEGORY_META_BUCKET)
                    .build();

            ListObjectsResponse res = s3Client.listObjects(listObjects);
            List<S3Object> objects = res.contents();

//            for (ListIterator iterVals = objects.listIterator(); iterVals.hasNext(); ) {
//                S3Object myValue = (S3Object) iterVals.next();
//                System.out.print("\n The name of the key is " + myValue.key());
//                System.out.print("\n The object is " + calKb(myValue.size()) + " KBs");
//                System.out.print("\n The owner is " + myValue.owner());
//            }
            return objects;
        } catch (S3Exception e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
        return null;
    }





    public ResponseInputStream<GetObjectResponse> interestCategoryMeta2(String bucket, String key){
        log.info("interestCategoryMeta : bucket = {}, key = {}",bucket,key);

        ResponseInputStream<GetObjectResponse> resInputStream =  s3Client.getObject(requestFor(bucket,key));
        log.info("여기까지1" );
        return resInputStream;
    }

    public List<S3Object> listBucketObjects2(String bucket) {

        try {
            ListObjectsRequest listObjects = ListObjectsRequest
                    .builder()
                    .bucket(bucket)
                    .build();

            ListObjectsResponse res = s3Client.listObjects(listObjects);
            List<S3Object> objects = res.contents();

//            for (ListIterator iterVals = objects.listIterator(); iterVals.hasNext(); ) {
//                S3Object myValue = (S3Object) iterVals.next();
//                System.out.print("\n The name of the key is " + myValue.key());
//                System.out.print("\n The object is " + calKb(myValue.size()) + " KBs");
//                System.out.print("\n The owner is " + myValue.owner());
//            }
            return objects;
        } catch (S3Exception e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
        return null;
    }






    private GetObjectRequest requestFor(String bucketname, String objKey) {

        log.info("S3 download requestFor bucket : {}, key : {}",bucketname,objKey);

        return GetObjectRequest.builder().bucket(bucketname).key(objKey).ifNoneMatch("").build() ;
    }

}
```