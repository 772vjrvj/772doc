```
//    다운로드 관련 사용X 버킷 리스트 & 버킷 다운 코드
//    @RequestMapping(value = "/downForm1", method = RequestMethod.GET)
//    public String downForm1(Model model) {
//
//        List<S3Object> objects =  s3Downloader.listBucketObjects();
//        List<String> resultList = new ArrayList<>();
//        for (ListIterator iterVals = objects.listIterator(); iterVals.hasNext(); ) {
//            S3Object myValue = (S3Object) iterVals.next();
//            System.out.print("\n The name of the key is " + myValue.key());
//            resultList.add(myValue.key());
//            System.out.print("\n The object is " + calKb(myValue.size()) + " KBs");
//            System.out.print("\n The owner is " + myValue.owner());
//        }
//        model.addAttribute("resultList", resultList);
//        return "awsS3Down1";
//    }
//
//    @RequestMapping(value = "/downForm2/{bucket}", method = RequestMethod.GET)
//    public String downForm2(Model model, @PathVariable String bucket) {
//
//        List<S3Object> objects =  s3Downloader.listBucketObjects2(bucket);
//        List<String> resultList = new ArrayList<>();
//        for (ListIterator iterVals = objects.listIterator(); iterVals.hasNext(); ) {
//            S3Object myValue = (S3Object) iterVals.next();
//            System.out.print("\n The name of the key is " + myValue.key());
//            resultList.add(myValue.key());
//            System.out.print("\n The object is " + calKb(myValue.size()) + " KBs");
//            System.out.print("\n The owner is " + myValue.owner());
//        }
//
//        model.addAttribute("bucket", bucket);
//        model.addAttribute("resultList", resultList);
//        return "awsS3Down1";
//    }
//
//    @RequestMapping(value = "/downForm", method = RequestMethod.GET)
//    public String downForm(Model model) {
//        return "awsS3Down";
//    }
//
//    //convert bytes to kbs
//    private long calKb(Long val) {
//        return val/1024;
//    }
//
//    @ResponseBody
//    @RequestMapping(value = "/downObject", method = RequestMethod.GET)
//    public ResponseEntity<byte[]> downObject(String filename, String bucket) throws Exception {
//        log.info("filename : {} ", filename);
//        log.info("bucket : {} ", bucket);
//        InputStream in = null;
//        ResponseEntity<byte[]> entity = null;
//
//        try {
//
//            in =  s3Downloader.interestCategoryMeta2(bucket, filename);
//            log.info("여기까지2" );
//            log.info("in : " + in);
//
//            String formatName = filename.substring(filename.lastIndexOf(".") + 1);
//
//            MediaType mType = getMediaType(formatName);
//
//            HttpHeaders headers = new HttpHeaders();
//
//            if (mType != null) {
//                headers.setContentType(mType);
//            } else {
//                filename = filename.substring(filename.indexOf("_") + 1);
//                headers.setContentType(MediaType.APPLICATION_OCTET_STREAM);
//                headers.add("Content-Disposition", "attachment; filename=\"" + new String(filename.getBytes("UTF-8"), "ISO-8859-1") + "\"");
//            }
//            entity = new ResponseEntity<byte[]>(IOUtils.toByteArray(in), headers, HttpStatus.CREATED);
//        } catch (Exception e) {
//            e.printStackTrace();
//            entity = new ResponseEntity<byte[]>(HttpStatus.BAD_REQUEST);
//        } finally {
//            in.close();
//        }
//        return entity;
//    }
//
//    private MediaType getMediaType(String formatName){
//        if(formatName != null) {
//            if(formatName.equals("JPG")) {
//                return MediaType.IMAGE_JPEG;
//            }
//
//            if(formatName.equals("GIF")) {
//                return MediaType.IMAGE_GIF;
//            }
//
//            if(formatName.equals("PNG")) {
//                return MediaType.IMAGE_PNG;
//            }
//        }
//        return null;
//    }
```