

| POC_WAS_35 | VIP 무료 영화 잔여 횟수       | O    | MbrMyPage.getMyMbrMovieCount     |
| ---------- | ----------------------------- | ---- | -------------------------------- |
| POC_WAS_36 | VIP 영화 잔여수, 임원여부조회 | O    | MbrMyPage.getMyMbrMovieCountInfo |
| POC_WAS_45 | 누적혜택금액조회              | O    | MbrMyPage.getMyUserDcAmt         |



#

SVC_MGMT_NUM

SVC_MGMT_NUM

서비스 관리 번호

ACK_MAST 테이블



결과

CNT











POC_WAS_20  신규-VIP Pick 당해년도 누적 사용횟수  O  DiyPlus.getR2PickYearUseCnt

input

|      Key       |   설명   |  예시  | 필수 |  타입  |  전송방식   |
| :------------: | :------: | :----: | :--: | :----: | :---------: |
|      name      |   이름   | 홍길동 |  O   | String | RequestBody |
| MBR_CARD_NUM10 | EBC_NUM1 |        |      |        |             |



|      Key       |  테이블명  |    Table_ID     | Column_ID |   컬럼명   | 필수 | 타입   |  전송방식   |
| :------------: | :--------: | :-------------: | :-------: | :--------: | ---- | ------ | :---------: |
| MBR_CARD_NUM10 | 멤버십카드 |  ACK_CARD_MAST  | EBC_NUM1  | 카드번호1  | O    | String | RequestBody |
|     CO_CD      |            | ACK_DC_PLAN_GRP |   CO_CD   | 제휴사코드 | O    | String | RequestBody |
|    GOODS_CD    |            | ACK_DC_PLAN_GRP | GOODS_CD  |  상품코드  | O    | String | RequestBody |
|                |            |                 |           |            |      |        |             |

CNT 당해년도 누적 사용횟수



|  Case   |                         Json Format                          | 설명 |
| :-----: | :----------------------------------------------------------: | :--: |
| Success | { coCd : "제휴사코드", coName : "제휴명칭", goodsName : "상품 명", hpnDtm: "등록 일시" } | 결과 |













POC_WAS_21  신규-VIP Pick 당월 사용여부  O  DiyPlus.getR2kPickMonthUseCnt

input

|      Key       |  테이블명  |    Table_ID     | Column_ID |   컬럼명   | 필수 | 타입   |  전송방식   |
| :------------: | :--------: | :-------------: | :-------: | :--------: | ---- | ------ | :---------: |
| MBR_CARD_NUM10 | 멤버십카드 |  ACK_CARD_MAST  | EBC_NUM1  | 카드번호1  | O    | String | RequestBody |
|     CO_CD      |            | ACK_DC_PLAN_GRP |   CO_CD   | 제휴사코드 | O    | String | RequestBody |
|    GOODS_CD    |            | ACK_DC_PLAN_GRP | GOODS_CD  |  상품코드  | O    | String | RequestBody |
|                |            |                 |           |            |      |        |             |



CO_CD : "제휴사코드"

CO_NAME : "제휴사명칭"

CNT : "상품 수량"



|  Case   |                         Json Format                          | 설명 |
| :-----: | :----------------------------------------------------------: | :--: |
| Success | { coCd : "제휴사코드", coName : "제휴명칭", goodsName : "상품 명", hpnDtm: "등록 일시" } | 결과 |















POC_WAS_22  신규-VIP Pick 사용내역  O  DiyPlus.getR2kPickUseList

input





|      Key       |  테이블명  |    Table_ID     | Column_ID |   컬럼명   | 필수 | 타입   |  전송방식   |
| :------------: | :--------: | :-------------: | :-------: | :--------: | ---- | ------ | :---------: |
| MBR_CARD_NUM10 | 멤버십카드 |  ACK_CARD_MAST  | EBC_NUM1  | 카드번호1  | O    | String | RequestBody |
|     CO_CD      |            | ACK_DC_PLAN_GRP |   CO_CD   | 제휴사코드 | O    | String | RequestBody |
|    GOODS_CD    |            | ACK_DC_PLAN_GRP | GOODS_CD  |  상품코드  | O    | String | RequestBody |
|                |            |                 |           |            |      |        |             |
|                |            |                 |           |            |      |        |             |



CO_CD : "제휴사코드"

CO_NAME : "제휴사명칭"

GOODS_CD: "상품코드"

GOODS_NAME: "상품 명"

HPN_DTM: "등록 일시"

|  Case   |                         Json Format                          | 설명 |
| :-----: | :----------------------------------------------------------: | :--: |
| Success | { coCd : "제휴사코드", coName : "제휴명칭", goodsName : "상품 명", hpnDtm: "등록 일시" } | 결과 |

