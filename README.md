# CrawlerApi_Aws

## 개요

구직을 하면서 매번 채용 사이트를 들어가는 일이 하루의 일과가 되었다. 매번 똑같이 보는 콘텐츠가 많아 지루하다고 느끼던 찰나, 크롤링을 해서 시각화 시킨 사이트나 하나 만들어 볼까? 하는 생각이 들었다. 하지만 자바 셀레늄으로 크롤링을 했던 코드를 다시 쓰는게 조금 지루하다는 생각이 들었다.

그래서 "크롤링하면 파이썬이지"하는 생각으로 개발을 시작했다. 처음에는 파이썬 크롤러로 로컬 저장소에 데이터를 저장했다가, 다시 웹 사이트로 가져가는 그림을 상상했다. 하지만 MSA를 공부하고 있는 와중이었기 때문에 개발에 적용을 시키고 싶었다. 그래서 오픈 API 서버를 구축하여 다른 서버에서 데이터를 받아오는 방법을 구축하는 방향으로 설계 했다.

사이트는 일단 로켓펀치를 대상으로 하였다. 이후 프로그래머스, 사람인 등으로 확장해 나갈 계획이고 중복 데이터를 제거하는 로직은 서버에서 구현할 예정이다.

<a href="https://www.notion.so/REST-API-AWS-EC2-2454edcbeaf8426381aec98f1143c17c">개발일지</a>

### 기술스택

- Python 3.x
- Flask
- Selenium
- AWS EC2

### 아키텍쳐 다이어그램
![Untitled](https://user-images.githubusercontent.com/66561524/110876664-492d6700-831b-11eb-8d96-e4747cb17740.png)
draw.io
[https://app.diagrams.net/?src=about#G1BlR1CSy57MiDlN9HXUpSvXTVGPRjVjD9](https://app.diagrams.net/?src=about#G1BlR1CSy57MiDlN9HXUpSvXTVGPRjVjD9)

▶️ **Request**

```
GET /get HTTP/1.1
Host: 54.180.98.206:8080
```

<h3>Parameter</h3>
<table>
	<thead>
		<tr>
			<th>Name</th>
			<th>Type</th>
			<th>Description</th>
			<th>Required</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>keywords</td>
			<td><code>String[]</code></td>
			<td>로켓펀치 채용 공고로 검색할 키워드</td>
		</tr>
	</tbody>
</table>

▶️ **Response**

<h3>Response</h3>
<table>
	<thead>
		<tr>
			<th>Name</th>
			<th>Type</th>
			<th>Description</th>
			<th>Required</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>clist</td>
			<td><code>Company[]</code></td>
			<td>당일날짜 기준으로 키워드로 로켓펀치에서 검색된 채용공고</td>
		</tr>
	</tbody>
</table>


⏺️ **Sample**

**Request**

```json
curl -v -X GET "https://54.180.98.206:8080/get"
    -d 'keywords=["soomgo","jpa"]'
```

**Response**

```json
{
	"name":"자비스앤빌런즈",
	"logo":"https://image.rocketpunch.com/company/14980/jobisnv_logo_1613702081.jpg?s=100x100&t=inside",
	"link":"https://www.rocketpunch.com/companies/jobisnv/jobs?utm_source=rocketpunch&utm_medium=advertisement&utm_campaign=job_ad&utm_content=job_page_title",
	"detail":"YOU WORK. WE HELP ",
	"jobs":[
						{
						"name":"백엔드 개발자",
						"stat":"5,000 - 10,000만원 / 경력",
						"date":"수시채용 03/09 수정",
						"link":"https://www.rocketpunch.com/jobs/77499/%EB%B0%B1%EC%97%94%EB%93%9C-%EA%B0%9C%EB%B0%9C%EC%9E%90?utm_source=rocketpunch&utm_medium=advertisement&utm_campaign=job_ad&utm_content=job_detail"
						}
				]
},
{
	"name":"라포랩스",
	"logo":"https://image.rocketpunch.com/company/135288/rapportlabs_logo_1606890359.png?s=100x100&t=inside",
	"link":"https://www.rocketpunch.com/companies/rapportlabs/jobs?utm_source=rocketpunch&utm_medium=advertisement&utm_campaign=job_ad&utm_content=job_page_title",
	"detail":"4050 여성을 위한 모바일 패션 커머스 플래폼을 만듭니다. ",
	"jobs":[
						{
						"name":"소프트웨어 엔지니어",
						"stat":"인턴, 신입, 경력",
						"date":"수시채용 02/20 수정",
						"link":"https://www.rocketpunch.com/jobs/85156/%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4-%EC%97%94%EC%A7%80%EB%8B%88%EC%96%B4?utm_source=rocketpunch&utm_medium=advertisement&utm_campaign=job_ad&utm_content=job_detail"
						}
				]
},
{
	"name":"숨고",
	"logo":"https://image.rocketpunch.com/company/17492/bravemobile_logo_1567392927.jpg?s=100x100&t=inside",
	"link":"https://www.rocketpunch.com/companies/bravemobile/jobs",
	"detail":"숨고와 위대한 여정을 같이 할 당신을 기다립니다. ",
	"jobs":[
						{
						"name":"[숨고] CX Specialist",
						"stat":"경력",
						"date":"04/01 마감 03/02 수정",
						"link":"https://www.rocketpunch.com/jobs/69824/%EC%88%A8%EA%B3%A0-CX-Specialist"
						}
				]
}
```
