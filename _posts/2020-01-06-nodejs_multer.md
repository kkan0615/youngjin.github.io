---
layout: post
title:  "[Node js] multer를 이용해 이미지 업로드하기"
image: ''
date:   2020-01-07 00:06:31
tags:
- node js
- javascript
description: 'Learn Multer with Node js!'
categories:
- javascript
- node js
---

## multer
multer은 node js에서 이미지를 업로드 할 때 편하게 해주는 기능을 하고 있다.
전송시 무조건 formdata로 보내야한다. 즉 json으로 보내서는 안된다.

## 설치
{% highlight bash %}
npm i multer
{% endhighlight %}

## 기본 사용 예제
{% highlight javascript %}
const multer = require('multer');
const path = require('path');
const upload = multer({
    storage: multer.diskStorage({
        // cb는 callback, cb대신 원하는 function 이름을 넣어도 된다. ex) done
        destination(req, file, cb) {
            cb(null, 'uploads/profile');
        },
        filename(req, file, cb) {
            const extention = path.extname(file.originalname); // 확장자 얻기
            const basename = path.basename(file.originalname, extention);
            cb(null, basename + Date.now() + extention);
        },
    }),
    limits: { fileSize: 20 * 1024 * 1024 } // 바이러스때문에 크기를 설정해주는 것이 좋다.
});

router.post('/uploadImage', upload.single('image'), (req, res, next) => {
    return res.json(req.file.filename);
});
{% endhighlight %}

### 파일 여러개 받기
{% highlight javascript %}
upload.array('image'); // image 라는 파일 정보를 배열로 얻는다
upload.array('image', 12); // image라는 파일 정보를 12개만 얻어서 배열로 얻는다.

router.post('/uploadImage', upload.array('image'), (req, res, next) => {
    let sendfile = req.files[0].filename;
    let fileArr = req.files.map(e => e.filename);
    return res.json({ sendfile, fileArr });
});
{% endhighlight %}

### fields를 이용하여 파일 받기
{% highlight javascript %}
upload.fields([{ name: 'avatar', maxCount: 1 }, { name: 'gallery', maxCount: 8 }])

router.post('/uploadImage', upload.fields([{ name: 'justImage', maxCount: 1 }, { name: 'goodImage', maxCount: 8 }]), (req, res, next) => {
    req.files['justImage'][0] // File 로 받기
    req.files['goodImage'] // Array로 받기
    return res.json({ babo: 'good' });
});
{% endhighlight %}

### 이외에도
{% highlight javascript %}
upload.none() // 텍스트 필드만 허용합니다.
upload.any() // 전달된 모든 형식의 파일을 받습니다. 모든 파일이 저장되기 때문에 주의하여 사용해야합니다.
{% endhighlight %}

## 파일에 들어 있는 정보
req.file 혹은 req.files를 했을 때 각 파일에 저장된 정보입니다.
Key	|| Description || Note
fieldname || 폼에 정의된 필드 명 || x
originalname || 사용자가 업로드한 파일 명 || x
encoding || 파일의 엔코딩 타입 || x
mimetype || 파일의 Mime 타입 || x
size || 파일의 바이트(byte) 사이즈 || x
destination || 파일이 저장된 폴더 || DiskStorage
filename || destination 에 저장된 파일 명 || DiskStorage
path || 업로드된 파일의 전체 경로 ||DiskStorage
buffer || 전체 파일의 Buffer || MemoryStorag

출처 : https://github.com/expressjs/multer/blob/master/doc/README-ko.md

## multer option 개체
Key || Description
dest or storage || 파일이 저장될 위치를 변경할 때 사용
fileFilter || 어떤 파일을 허용할지 제어할 때 사용
limits || 업로드 될 파일의 크기를 제한 할때 사용
preservePath || 파일의 base name 대신 보존할 파일의 전체 경로
filename || 파일의 이름을 변경 할 때 사용

### 예시
{% highlight javascript %}
const upload = multer({
    storage: multer.diskStorage({
        // cb는 callback, cb대신 원하는 function 이름을 넣어도 된다. ex) done
        destination(req, file, cb) {
            cb(null, 'uploads/profile');
        },
        filename(req, file, cb) {
            const extention = path.extname(file.originalname);
            const basename = path.basename(file.originalname, extention);
            cb(null, basename + Date.now() + extention);
        },
        fileFilter(req, file, cb) {

        // 이 함수는 boolean 값과 함께 `cb`를 호출함으로써 해당 파일을 업로드 할지 여부를 나타낼 수 있습니다.
        // 이 파일을 거부하려면 다음과 같이 `false` 를 전달합니다:
        cb(null, false)

        // 이 파일을 허용하려면 다음과 같이 `true` 를 전달합니다:
        cb(null, true)

        // 무언가 문제가 생겼다면 언제나 에러를 전달할 수 있습니다:
        cb(new Error('I don\'t have a clue!'))
        },
    }),
    limits: { fileSize: 20 * 1024 * 1024 }
});
{% endhighlight %}

#### limits 관련하여 추가 설명
속성 || 설명 || 기본값
fieldNameSize || 필드명과 사이즈 최대값 || 100 bytes
fieldSize | 필드값 사이즈의 최대값 || 1MB
fields || 파일형식이 아닌 필드의 최대 개수 ||무제한
fileSize || multipart 형식 폼에서 최대 파일 사이즈(bytes) || 무제한
files || multipart 형식 폼에서 파일 필드의 최대 개수 || 무제한
parts || multipart 형식 폼에서 fields와 파일을 합친 부분의 최대 개수 || 무제한
headerPairs || multipart 형식 폼에서 파싱할 헤더의 key=>value 쌍의 최대 개수 || 2000

## 이미지 로드하기
{% highlight javascript %}
/** app.js */
// app.js에서 아래 라인을 추가해준다.
app.use('/profileImage', express.static('uploads/profile'));
{% endhighlight }

{% highlight javascript %}
/** client 에서 사용법 */
<v-img :src="'http://localhost:8001/profileImage/' + imgName"></v-img>
{% endhighlight }

## multer과 AWS 같이 사용해보기
{% highlight javascript %}
const multer = require('multer');
const path = require('path');
const AWS = require('aws-sdk');
const multerS3 = require('multer-s3');

AWS.config.update({
  region: region,
  accessKeyId: S3 Acess Key ID를 적어주세요,
  secretAccessKey: S3 Secret Access Key를 적어주세요,
});

const upload = multer({
    storage: multerS3({
        s3: new AWS.S3(),
        bucket: bucketname을 적어주세요,
        key(req, file, cb) {
            const newName = 'original/' + Date.now() + path.basename(file.originalname);
            cb(null, newName);
        },
    }),
    limit: { fileSize: 20 * 1024 * 1024 },
});
router.post('/images', isLoggedIn, upload.sigle('image'), (req, res) => {
    res.json(req.file.filename;
});
{% endhighlight %}

## Reference
1. https://github.com/expressjs/multer/blob/master/doc/README-ko.md [ Multer github ]
2. https://www.npmjs.com/package/multer [ multer npm ]

## 끝으로
댓글 혹은 이메일을 통해 피드백 받습니다. 혹시 예시가 더 필요하시면 제가 추가적으로 올리도록 하겠습니다.
