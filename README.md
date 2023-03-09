# File-Uploads
nomard Youtube Code Challenge 9

```
(1) 파일을 업로드하기 위해 multer를 사용합니다. multer는 파일 업로드를 위해 사용되는 multipart/form-data를 다루기 위한 node.js의 미들웨어입니다.

(2) Views
  form(method="POST" enctype="multipart/form-data")
  Multer는 multipart (multipart/form-data)인 form에서만 작동합니다. form의 enctype(encoding type)을 multipart/form-data로 설정하여 파일을 백엔드로 보낼 수 있게 하면 됩니다.
  
(3) multer의 diskStorage 엔진을 사용하여 더 상세하게 저장 위치와 파일명을 설정할 수 있습니다.
  ![01](https://user-images.githubusercontent.com/88027485/224108734-360d471c-fdd2-430d-a8cd-38db6b3dd3e6.png)
  파일이 저장될 디렉터리를 uploads 폴더로 지정하고, 저장될 때 파일명을 “원 파일명-현재 시각”으로 설정합니다.
  const uploadFiles = multer({ storage })
  사용자가 보낸 파일이 위에서 설정한 storage에 저장되도록 uploadFiles 미들웨어를 설정합니다.
  
(4) app.route("/").post(uploadFiles.single("text"), postText)
  uploadFiles 미들웨어를 /로 보내는 POST 라우트에 적용합니다.
  하나의 파일만 업로드하기 위해 middleware.single(“form input의 name”)을 사용합니다.
  이렇게 하면 multer 미들웨어가 template의 input에서 오는 .txt 파일을 받아서 uploads 폴더에 저장하고, 그 파일 정보를 controller인 postText에 전달합니다.

(5) postText 컨트롤러에서는 req.file로 사용자가 업로드한 파일 정보를 받아올 수 있습니다.
  const { filename } = req.file
  res.redirect(`/read/${filename}`)
  받아온 파일을 /read/${filename}로 redirect 합니다.
  따라서 사용자가 업로드한 .txt 파일은 /read/:id라우트로 보내져 파일 내용이 화면에 표시됩니다.

(6)app.get("/read/:id", showText)
  showText 컨트롤러에서는 fs.readFile()를 사용하여 파일을 열고 사용자에게 내용을 표시합니다.
  ![02](https://user-images.githubusercontent.com/88027485/224108740-7758b38b-d45d-4845-b0e8-22ae0e6493ef.png)
  req.params으로 각 파일의 id를 가져오고 파일의 path 값을 설정합니다.
  fs.readFile(“파일 주소”, (err, 파일 내용))을 사용하면 파일 주소에 해당하는 파일의 내용을 불러올 수 있습니다.
  res.send()로 파일의 data를 보내면 .txt 파일의 내용이 화면에 표시됩니다.

(7)app.route("/").get(getText)
  파일을 업로드하기 위한 form을 렌더 하기 위해 /로 GET을 요청합니다.
  getText 컨트롤러에 fs.readdir()를 사용하여 디렉터리 안에 있는 파일 목록을 불러옵니다.
  참고 사항
  fs.readdir(파일 디렉터리 (err, 디렉터리에 있는 파일))를 사용하면 디렉터리 안에 있는 파일을 불러올 수 있습니다. 
  pug 템플릿에서 iterate와 함께 쓰면 디렉터리 안의 파일 목록을 구현할 수 있습니다.
  ![03](https://user-images.githubusercontent.com/88027485/224108749-dea736eb-78a0-4183-b7b2-728e14fe77ac.png)
  uploads 폴더에서 불러온 data를 home.pug에 렌더 합니다.

(8) 홈 화면에 업로드한 모든 .txt 파일의 파일명을 나열하기 위해 getText 컨트롤러에서 보낸 data를 home.pug에서 사용합니다.
  each filename in fileNameList
  iterate 구문으로 파일명을 나열하면 됩니다.
  a(href="/read/" + filename) #{filename.split("-")[0]}
  각 파일명에 /read/:id에 대한 링크를 주려면 위의 코드처럼 작성하면 됩니다.
```
