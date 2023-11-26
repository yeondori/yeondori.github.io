---
title: "[프로젝트]원티드 프리온보딩 과제 수행 02 기능 구현"
excerpt: "원티드 프리온보딩 과제 수행 과정 기록"
categories: [Project, Wanted-pre-onboarding]
tags: [Spring]
date: 2023-10-18
last_modified_at: 2023-10-18
render_with_liquid: false
---
# [프리온보딩 인턴십 수행과제](https://bow-hair-db3.notion.site/1850bca26fda4e0ca1410df270c03409)

프로젝트는 [🚀여기🚀](https://github.com/yeondori/wanted-pre-onboarding-backend)에서 확인할 수 있다.
트러블 슈팅에 관해서는 [[프로젝트]원티드 프리온보딩 과제 수행 03](https://yeondori.github.io/posts/pre-onboarding-03/) 여기서 한번에 다루고 있다!

## 과제 수행 과정

1. 구조 설계
2. 환경 구성, H2 데이터 베이스 연동
3. 엔티티 생성 및 초기 데이터 설정
4. Repository, Service, Controller 생성
5. 기능 추가
   - 공고 등록
   - 공고 상세보기
   - 상세보기에서 지원 기능 추가
   - 검색 기능
   - 공고 삭제
   - 공고 수정
   - 조회 기능
   - 상세보기에서 회사가 올린 다른 공고 보기
6. Github 업로드
7. README.md 작성
8. 제출 😂😂

## 5.기능 추가

기능 추가 과정을 풀어보기에 앞서 다시 한번 정리하고 넘어가자면 이렇게 설계해두었다.
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/0b3ce17b-6cef-4772-9cb1-fc45a145cfb1)

**CompanyController**에서는 채용공고 등록, 수정, 삭제 기능을 제공하며 
**PostController**에서는 채용공고 목록 반환, 채용공고 상세, 채용공고 검색, 채용공고 지원 기능을 제공한다.

기능을 추가한 순서대로 작성하려고 했으나 사실상 수정의 수정을 거듭했기 때문에 저 순서대로 정리하는 것보다는 순차적으로 설명하는 게 더 편할 것 같다. 

### 홈 화면

별도로 매핑해주지 않으면 "/"이 요청될 때 index.html을 반환한다.

**index.html**
```java
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>채용공고 서비스</title>
</head>
<body>
<h1>채용공고 서비스 홈화면</h1>

<!-- 검색 폼 추가 -->
<form action="/jobpostings/search" method="get" accept-charset="UTF-8">
    <label>
        <input type="text" name="keyword" placeholder="검색어를 입력하세요.">
    </label>
    <button type="submit">검색</button>
</form>
<br>
<!-- 기업 등록 버튼 -->
<a href="/companies"><strong>기업회원</strong></a>
<!-- 채용공고 목록 버튼 -->
<a href="/jobpostings"><strong>일반회원</strong></a>
</body>
</html>
```

아래 부분만 간략히 보면 검색 폼의 버턴을 누르면 "/jobpostings/search" URI의 GET 메소드가 요청되고,

기업 등록 버튼은 "/companies" 를, 채용공고 목록 버튼은 "/jobpostings" 으로 이동한다. 

자세한 건 차차 다뤄보자!

### 기업 전용 기능

참고로 CompanyController는 @RequestMapping(value = "/companies") 어노테이션이 있다.
컨트롤러의 루트 경로가 "/companies" 라는 뜻!

#### 기업 홈화면, 기업 페이지

```java
//기업 홈화면
@GetMapping("")
public String home() {
return "companies/companiesHome";
}

 //기업 회원 페이지
 @GetMapping("/{companyId}")
 public String getJobPostingsByCompany(@PathVariable Long companyId, Model model) {
     Optional<Company> company = companyService.findById(companyId);

     if (!company.isPresent()) {
         return "Company ID가 존재하지 않습니다.";
     }

     List<JobPosting> jobPostings = postService.findByCompanyId(company.get().getId());

     model.addAttribute("jobPostings", jobPostings);
     model.addAttribute("company", company.get());

     return "companies/postList";
 }
```

홈화면에서는 기업ID를 입력받아 기업 회원페이지를 반환한다. 인증 절차가 생략되어 있는 프로젝트이므로 간단하게 구성했다.

입력 받은 회사의 ID가 존재하지 않는 경우 문구가 출력되며, 유효한 회사 ID를 입력한 경우에는 해당 회사의 채용공고를 리스트에 담아 model에 추가한 뒤 회원 페이지 html을 반환한다.

[실행 예시]
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/6933e174-874d-4940-9ca4-0f7ca2672fa9)

#### 채용공고 등록

채용공고 등록 방식은 스프링 입문 강의에서 배운 createForm() 으로 폼을 생성한 뒤, 정보를 넘겨주는 방식을 참고하여 구현했다. 

**PostForm**
```java
public class PostForm {

    private Long companyId;
    private String jobPosition;
    private Long recruitmentCompensation;
    private String recruitmentDetails;
    private String technologyUsed;

    public Long getCompanyId() {return companyId;}
    public void setCompanyId(Long companyId) {this.companyId = companyId;}

    public String getJobPosition() {return jobPosition;}
    public void setJobPosition(String jobPosition) {this.jobPosition = jobPosition;}

    public Long getRecruitmentCompensation() {return recruitmentCompensation;}
    public void setRecruitmentCompensation(Long recruitmentCompensation) {this.recruitmentCompensation = recruitmentCompensation;}

    public String getRecruitmentDetails() {return recruitmentDetails;}
    public void setRecruitmentDetails(String recruitmentDetails) {this.recruitmentDetails = recruitmentDetails;}

    public String getTechnologyUsed() {return technologyUsed;}
    public void setTechnologyUsed(String technologyUsed) {this.technologyUsed = technologyUsed;}
}
```
회사ID, 채용포지션, 채용보상금 등 채용공고에 필요한 필드들을 정의해주었고 getter, setter를 추가해주었다.

**CompanyController**

```java
 //채용공고 등록 폼
 @GetMapping("/{id}/new")
 public String showJobPostingForm(@PathVariable Long id, Model model) {
     model.addAttribute("companyId", id);
     return "companies/createdPostForm"; 
 }

 //채용공고 등록
 @PostMapping("/{id}/new")
 public String create(PostForm form) {
     Company company = companyService.findById(form.getCompanyId()).orElse(null);

     if (company == null) {
         return "Company ID가 존재하지 않습니다.";
     }
     Long companyId = form.getCompanyId();

     JobPosting newPost = new JobPosting();

     newPost.setCompany(company);
     newPost.setJobPosition(form.getJobPosition());
     newPost.setRecruitmentDetails(form.getRecruitmentDetails());
     newPost.setTechnologyUsed(form.getTechnologyUsed());
     newPost.setRecruitmentCompensation(form.getRecruitmentCompensation());

     postService.save(newPost);

     return "redirect:/companies/" + companyId;
 }
```

1. "/companies/{id}/new" GET -> CompanyID를 모델에 넘겨준 후 template 디렉토리 내부에 있는 companies/createdPostForm.html 호출

2. createdPost.html에서 해당 필드들을 입력받은 뒤 같은 URI로 POST 요청

3. "/companies/{id}/new" Post -> html에서 작성한 PostForm 객체를 받아온다.

4. Company ID를 검증하고(이 부분은 등록 URI가 "/companies/new"일 때 작성된 부분인데 현재는 불필요할 것 같다), 새로운 JobPosting 객체를 생성한다.

5. 새로운 JobPosting 객체에 form의 내용을 주입한 뒤 저장한다. 

6. 회사의 페이지(해당 회사가 올린 채용공고 목록)를 반환한다.

[실행 예시]
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/4d31298d-1198-403c-8cc8-ffe1de65fa0b)

#### 채용공고 삭제

**postList.html** 
```html
<form th:action="@{'/companies/deleteJobPosting/' + ${jobPosting.id}}" method="post">
<input type="hidden" name="_method" value="delete" />
<button type="submit" onclick="return confirm('정말로 이 작업을 삭제하시겠습니까?')">삭제</button>
</form>
```
기업 페이지의 삭제버튼을 살펴보면, 삭제 버튼을 누를 경우 "/companies/deleteJobPostings/{채용공고Id}"에 POST 메소드로 요청이 되는 것 같지만 hidden 타입으로 DELETE 요청이 전송된다.

**CompanyController**
```java
//채용공고 삭제
@DeleteMapping("/deleteJobPosting/{postId}")
public String deleteJobPosting(@PathVariable Long postId) {
    Long companyId = postService.findCompanyIdById(postId);
    postService.deleteById(postId);
return "redirect:/companies/" + companyId;
}
```

findCompanyIdById는 채용공고의 Id로 해당 채용공고의 Company Id를 반환하는 메소드로, PostService에 다음과 같이 구현해 두었다.
레포지토리에 정의되지 않았는데 괜찮은가... 하는 의문 

**PostService**
```java
public Long findCompanyIdById(Long id) {
Optional<JobPosting> jobPosting = findById(id);
if (!jobPosting.isPresent()) {
throw new EntityNotFoundException("Job Posting not found for id: " + id);
}
return jobPosting.get().getCompany().getId();
}
```

삭제 후에는 기업 페이지를 반환한다.

[실행 예시]
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/2e4182bd-653d-4afa-a2c3-4b780b87950e)

#### 채용공고 수정

**postList.html**
```html
<form th:action="@{'/companies/editJobPosting/' + ${jobPosting.id}}" method="get">
   <button type="submit">수정</button>
</form>
```
수정 버튼 또한 이러한 방식으로 "/companies/editJobPosting/{채용공고Id}" 에 GET 요청을 보낸다.

**CompanyController**
```java
//채용공고 수정
@GetMapping("/editJobPosting/{jobPostingId}")
public String getEditJobPostingForm(@PathVariable Long jobPostingId, Model model) {
    Optional<JobPosting> jobPosting = postService.findById(jobPostingId);

      if (!jobPosting.isPresent()) {
         return "채용 공고를 찾을 수 없습니다.";
      }
      
      model.addAttribute("jobPosting", jobPosting.get());
      
      return "companies/editJobPosting";
}
```
요청을 받은 getEditJobPostingForm 메소드는 해당 채용공고를 찾아 모델에 추가한 뒤 수정 페이지를 반환한다.

**editJobPosting.html**
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
   <meta charset="UTF-8">
   <title>채용공고 수정</title>
</head>
<body>
<h1>채용공고 수정</h1>

<form th:action="@{'/companies/updateJobPosting/' + ${jobPosting.id}}" method="post">
   <input type="hidden" name="id" th:value="${jobPosting.id}" />

   <label for="jobPosition">채용 포지션:</label>
   <input type="text" id="jobPosition" name="jobPosition" th:value="${jobPosting.jobPosition}" />
   <br />

   <label for="recruitmentCompensation">채용 보상금:</label>
   <input type="text" id="recruitmentCompensation" name="recruitmentCompensation" th:value="${jobPosting.recruitmentCompensation}" />
   <br />

   <label for="recruitmentDetails" style="vertical-align: top;">채용 내용:</label>
   <textarea id="recruitmentDetails" name="recruitmentDetails" rows="5" cols="50">
    [[${jobPosting.recruitmentDetails}]]</textarea>
   <br />

   <label for="technologyUsed">사용 기술:</label>
   <input type="text" id="technologyUsed" name="technologyUsed" th:value="${jobPosting.technologyUsed}" />
   <br />

   <button type="submit">수정</button>
</form>

</body>
</html>
```
수정 페이지의 html을 자세히 볼 필요는 없고 

`<form th:action="@{'/companies/updateJobPosting/' + ${jobPosting.id}}" method="post">` 

여길 살펴보면 채용공고 수정하기 버튼을 누르면 thymeleaf 선생님께서 수정한 정보들을 가지고  "/companies/updateJobPosting/{채용공고Id}"에 POST 메소드로 요청을 보낸다.

**CompanyController**
```java
 //채용공고 수정결과 업데이트
 @PostMapping("/updateJobPosting/{jobPostingId}")
 public String updateJobPosting(@PathVariable Long jobPostingId, @ModelAttribute JobPosting updatedJobPosting) {
     postService.updatePost(updatedJobPosting);
     Long companyId = updatedJobPosting.getCompany().getId();
     return "redirect:/companies/" + companyId;
 }
```

그러면 다시 이 요청을 매핑한 updateJobPosting가 update를 수행하고 기업 페이지를 반환하게 된다.

참고로 PostService의 updatePost는 아래와 같다.

**PostService**
```java
public JobPosting updatePost(JobPosting updatedJobPosting) {

     Optional<JobPosting> targetJobPosting = postRepository.findById(updatedJobPosting.getId());

     if (targetJobPosting.isPresent()) {
         Company company = targetJobPosting.get().getCompany();
         updatedJobPosting.setCompany(company);
     }
     return postRepository.save(updatedJobPosting);
}
```
++ 작성 중에 발견한 오류 updatePost void네...?

```java
public void updatePost(JobPosting updatedJobPosting) {

        Optional<JobPosting> targetJobPosting = postRepository.findById(updatedJobPosting.getId());

        if (targetJobPosting.isPresent()) {
            Company company = targetJobPosting.get().getCompany();
            updatedJobPosting.setCompany(company);
        }
        postRepository.save(updatedJobPosting);
    }
```

넘겨받은 jobPosting이 실제로 존재하는지 확인한 후에 존재한다면 Company 정보를 받아와야 한다. 
updatJobPositng에는 회사 정보가 없기 때문에 이 작업을 해주지 않으면 Company 부분이 null이 된다.

정리하면 
1. 수정 버튼을 클릭 -> "/companies/editJobPosting/{채용공고Id}" GET 

2. 컨트롤러의 getEditJobPostingForm에서 채용 공고를 모델에 담아 수정 페이지 반환

3. 수정 페이지에서 수정한 정보들을 담아 "/companies/updateJobPosting/{채용공고Id}" POST 

4. 컨트롤러의 updateJobPosting에서 update 수행 후 기업 페이지 반환

[실행 예시]
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/8f88aabd-a573-4dcb-bfdf-25810d7b4e46)
(참고로 예시에서 수정화면은 수정 전 화면이다)

### 사용자 기능

사용자 기능이라는 명명이 적절한 지는 모르곘으나 현재 구현된 프로젝트 수준에서는 회원 전용 기능이 별도로 존재하지 않으므로 채용공고 목록, 상세보기, 지원 등의 프로세스를 사용자 기능으로 명명하겠다.

참고로 모든 기능은 PostController에서 작동하는데 컨트롤러에는 @RequestMapping(value = "/jobpostings") 어노테이션이 있다.

#### 채용공고 목록 

**PostController**
```java
//채용 공고 목록
@GetMapping("")
public String retrieveAllPosts(Model model){
        List<JobPosting> jobPostings=postService.findAll();
        List<JobPostingDTO> jobPostingDTOS=mapToJobPostingDTOList(jobPostings);
        model.addAttribute("jobPostingDTOs",jobPostingDTOS);
        return"jobpostings/home";
}

private List<JobPostingDTO> mapToJobPostingDTOList(List<JobPosting> jobPostings) {
   List<JobPostingDTO> jobPostingDTOs = new ArrayList<>();
   
   for (JobPosting jobPosting : jobPostings) {
      JobPostingDTO dto = new JobPostingDTO();
      dto.setId(jobPosting.getId());
      dto.setCompanyName(jobPosting.getCompany().getName());
      dto.setCompanyCountry(jobPosting.getCompany().getCountry());
      dto.setCompanyRegion(jobPosting.getCompany().getRegion());
      dto.setJobPosition(jobPosting.getJobPosition());
      dto.setRecruitmentCompensation(jobPosting.getRecruitmentCompensation());
      dto.setTechnologyUsed(jobPosting.getTechnologyUsed());
   
      jobPostingDTOs.add(dto);
   }
   
   return jobPostingDTOs;
}
```

"/jobpostings" 요청을 받으면 모든 채용공고 목록을 반환한다. 이때 Company와 JobPosting 엔티티는 서로를 참조하는 양방향 연결관계이므로 
순환 참조 문제가 발생하는데, 이때 JsonIgnore 어노테이션을 이용해 반환하지 않도록 해주었다. 

문제는 채용 내용이라는 컬럼은 상세보기를 누른 경우에만 보여야 하므로 DTO 클래스를 별도로 생성해주었다. 

**JobPostingDTO**
```java
public class JobPostingDTO {
private Long id;
private String companyName;
private String companyCountry;
private String companyRegion;
private String jobPosition;
private Long recruitmentCompensation;
private String technologyUsed;

    //getter, setter 생략
}
```
요구사항에서 명시된 항목들만 담겨져 있다.

mapToJobPostingDTOList 메소드에서 JobPostings 리스트를 JobPostingDTO 리스트로 매핑해준다.

매핑 후 모델에 DTO 리스트를 담아 jobpostings/home.html 에 반환한다.

[실행 예시]
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/67cb3715-2bf0-485f-83bb-9d1d2b5ada8d)


#### 채용공고 상세보기 


**PostController**
```java
 // 채용공고 상세
 @GetMapping("/{id}")
 public String retrieveDetails(@PathVariable Long id, Model model) {
     Optional<JobPosting> selectedPost = postService.findById(id);
     if (!selectedPost.isPresent()) {
         throw new UserNotFoundException(String.format("ID[%s] is not found", id));
     }
     model.addAttribute("jobPosting", selectedPost.get());

     List<Long> jobPostingIdList = selectedPost.get().getCompany().getJobPostingIdList();
     jobPostingIdList.remove(id); // 현재 채용공고 ID를 삭제

     if (jobPostingIdList.isEmpty()) {
         model.addAttribute("anotherPosts", "None");
     } else {
         model.addAttribute("anotherPosts", jobPostingIdList);
     }

     return "jobpostings/postDetails";
 }
```

상세보기 클릭 시에는 채용 내용과 해당 공고를 올린 회사의 다른 공고가 출력되어야 한다.

이때는 DTO가 아닌 JobPosting 객체를 모델에 담아주며, 회사의 다른 공고는 별도의 작업을 거친다.

해당 포스트에서 회사 정보를 가져온 뒤 getJobPostingIdList() 메소드로 회사의 공고들을 가져오는데, 해당 메소드는 Company 클래스에 다음과 같이 구현되어 있다.

**Company**
```java
public List<Long> getJobPostingIdList() {
   List<Long> idList = new ArrayList<>();
   for (JobPosting jobPosting : jobPostingList) {
   idList.add(jobPosting.getId());
   }
return idList;
}
```

Company의 jobPostingList에서 id를 가져와 id 리스트에 add 한 뒤, id 리스트를 반환한다.

이러한 방식으로 가져온 id 리스트에서 현재 채용공고의 id는 제거한 뒤에 리스트가 비어있다면 "None"을, 비어있지 않다면 리스트를 모델에 담아 상세페이지로 반환한다. 

**postDetails.html**
```html
<h3 th:utext="'<strong>채용내용:</strong> ' + ${jobPosting.recruitmentDetails}"></h3>
<h3><strong>다른 공고:</strong></h3>
  <!-- anotherPosts가 "None"인 경우에 메시지 표시 -->
  <h3 th:if="${anotherPosts == 'None'}"> 존재하지 않습니다.</h3>
  <!-- anotherPosts가 "None"이 아닌 경우에는 하이퍼링크 목록 표시 -->
  <ul th:unless="${anotherPosts == 'None'}">
    <!-- "다른 공고:" 메시지 스타일 변경 -->
    <li th:each="postId : ${anotherPosts}">
      <a th:href="@{'/jobpostings/' + ${postId}}">게시물 ID: <span th:text="${postId}"></span></a>
    </li>
  </ul>
```

첫줄에 채용내용과, 둘째줄 이하의 다른 공고를 확인할 수 있다. anotherPost가 "None"인 경우에는 메시지를 출력하고, anotherPost가 리스트 형식인 경우 하나씩 출력한다.

참고로 이떄는 "postId: {채용공고 Id}" 에 상세 페이지로 갈 수 있는 URI("/jobpostings/{채용공고 Id}")가 하이퍼링크로 걸려있다. 

페이지 하단에는 지원하기 버튼도 있는데 이건 아래에서 설명하곘다.

[실행 예시]
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/ff97d71c-46ad-4b17-a8ba-ea7a03e3dc1b)

#### 채용공고 지원

**postDetails.html**
```html
<div class="apply-form">
  <h4>해당 공고에 관심이 있으신가요?</h4>
  <form th:action="@{'/jobpostings/' + ${jobPosting.id} + '/apply'}" method="post" onsubmit="return confirm('해당 공고에 지원하시겠습니까?\n지원은 1회만 가능하며 철회할 수 없습니다.')">
    <label for="memberId"></label>
    <input type="text" id="memberId" name="memberId" required placeholder="지원자 ID를 입력하세요"/>
    <button type="submit">지원하기</button>
  </form>
</div>
```

방금 전에 언급한 지원하기 버튼이 바로 이거다. 지원 시에는 "/jobpostings/{채용공고 Id}/apply" 라는 URI에 POST로 전송하는데, 이때 memberId를 같이 넘겨준다.


**PostController**
```java
// 채용공고 지원
public static final int SUCCESS = 1;
public static final int MEMBER_NOT_FOUND = 2;
public static final int POST_NOT_FOUND = 3;
public static final int ALREADY_APPLIED = 4;

    @PostMapping("/{id}/apply")
    public String applyForJob(@PathVariable Long id, @RequestParam String memberId, Model model) {
        Long applicantId = Long.parseLong(memberId);
        Map<String, Object> response = new HashMap<>();
        int status = SUCCESS;

        if (memberService.findById(applicantId).isEmpty()) {
            status = MEMBER_NOT_FOUND;
        } else {
            Optional<JobPosting> selectedPost = postService.findById(id);
            if (!selectedPost.isPresent()) {
                status = POST_NOT_FOUND;
            } else {
                JobPosting jobPosting = selectedPost.get();
                Member applicant = memberService.findById(applicantId).get();
                if (applicant.getAppliedPosting() != null) {
                    status = ALREADY_APPLIED;
                } else {
                    applicant.setAppliedPosting(jobPosting);
                    memberService.save(applicant);
                    postService.save(jobPosting);
                }
            }
        }
        model.addAttribute("jobPostingId", id);
        model.addAttribute("applicantId", applicantId);
        model.addAttribute("applyStatus", status);
        return "jobpostings/applyResult";
    }
```

여기서 id는 채용공고 id를 의미하며, URI에서 @PathVariable로 가져온다. 지원하기 버튼에서 넘겨받은 memberId는 String의 형태로 @RequestParam에서 받아온다.
지원 상태코드를 명명해 주었고, 지원자는 하나의 공고에 1회 지원이 가능하므로 경우는 총 네 가지이다.

1. 지원 이력이 없는 사용자가 지원한 경우(성공)
2. 사용자를 찾을 수 없는 경우(실패)
3. 채용공고를 찾을 수 없는 경우(실패)
4. 지원 이력이 있는 사용자가 지원한 경우(실패)

이러한 상태를 status에 담아 모델에 추가해주었고, 작업에 필요한 지원자 Id와 채용공고 Id까지 모델에 추가하여 "jobpostings/applyResult"를 반환한다.

++ response 부분은 초기 코드이며 초기에는 위의 메소드에서 메시지를 바로 반환해주었다. 그러나 이를 Html에서 다루고 싶어서 현재의 코드로 수정했고, 불필요한 부분이라 삭제해 주었다.

**applyResult.html**
```html
<!-- applyResult.html -->
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>지원 결과</title>
</head>
<body>
<h1> 지원 결과</h1>
<div id="resultMessage">
    <h3 th:if="${applyStatus == 2}">존재하는 회원이 아닙니다.</h3>
    <h3 th:if="${applyStatus == 3}">채용공고가 존재하지 않습니다.</h3>
    <h3 th:if="${applyStatus == 4}">이미 채용공고에 지원하여, 지원이 불가합니다.</h3>
</div>

<!-- 공고 및 지원자 ID 출력 -->
<div th:if="${applyStatus == 1}">
    <h3>지원이 완료되었습니다. 지원하신 내역은 다음과 같습니다.</h3>
    <table style="border: 2px solid #ddd; background-color: #f5f5f5; padding: 10px;">
        <tr>
            <td style="font-weight: bold;">채용공고 ID:</td>
            <td><span th:text="${id}"></span></td>
        </tr>
        <tr>
            <td style="font-weight: bold;">지원자 ID:</td>
            <td><span th:text="${applicantId}"></span></td>
        </tr>
    </table><br>
</div>

<!-- 채용목록 둘러보기 및 채용공고로 돌아가기 버튼 추가 -->
<div class="job-details">
    <a th:href="@{'/jobpostings/' + ${id}}">채용공고로 돌아가기</a><br>
    <a th:href="@{/jobpostings}">채용목록 둘러보기</a>
</div>
</body>
</html>
```

이런 방식으로 지원 결과 폼을 생성해주었다.

[실행 예시]
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/db3f6b23-1a51-4224-a6bf-691bd964da1d)
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/ba5201dd-af30-4894-bf44-1c894907f878)

#### 채용공고 검색

홈 화면에 검색 폼을 통해 검색이 가능하도록 검색 기능을 구현했다. 

**index.html**
```java
<form action="/jobpostings/search" method="get" accept-charset="UTF-8">
    <label>
        <input type="text" name="keyword" placeholder="검색어를 입력하세요.">
    </label>
    <button type="submit">검색</button>
</form>
```
참고로 홈 화면의 이 부분이 검색 폼이고, 입력받은 키워드를 가지고 "/jobpostings/search"에 GET 요청을 전송한다.

**PostController**
```java
// 채용공고 검색
@GetMapping("/search")
public String searchJobPostings(@RequestParam("keyword") String keyword, Model model) {
   if (keyword.isEmpty()) {
       return "redirect:/jobpostings";
   } else {
   List<JobPosting> searchResults = postService.findBySearchKeyword(keyword);
   model.addAttribute("keyword", keyword);
   
      if (searchResults.isEmpty()) {
          return "jobpostings/noResults"; // 검색 결과가 없는 경우에 대한 뷰
      } else {
          List<JobPostingDTO> searchResultsDTO = mapToJobPostingDTOList(searchResults);
          model.addAttribute("searchResults", searchResultsDTO);

          return "jobpostings/searchResults"; // 검색 결과가 있는 경우에 대한 뷰
      }
  }
}
```
해당 URI와 매핑된 searchJobPostings가 RequestParam을 통해 keyword를 받아오고, 세 가지 경우로 나눠 처리된다.

1. 키워드가 비어 있는 경우 -> 채용공고 목록 반환
2. 키워드로 검색 시 검색 결과가 없는 경우 -> 검색 결과 없는 경우의 뷰 반환
3. 키워드로 검색 시 검색 결과가 있는 경우 -> 검색 결과가 있는 경우의 뷰 반환

여기서 findBySearchKeyword는 PostService에 다음과 같이 정의되어 있으며,

**PostService**
```java
public List<JobPosting> findBySearchKeyword(String keyword) { return postRepository.findBySearchKeyword(keyword);}
```

PostRepository에 구현되어 있다.

**PostRepository**
```java
 @Query("SELECT p FROM JobPosting p " +
        "WHERE p.company.name LIKE %:keyword% " +
        "OR p.company.country LIKE %:keyword% " +
        "OR p.company.region LIKE %:keyword% " +
        "OR p.jobPosition LIKE %:keyword% " +
        "OR p.technologyUsed LIKE %:keyword%")
    List<JobPosting> findBySearchKeyword(String keyword);
```
쿼리를 살펴보면 keyword를 포함하고 있는지 여부를 모든 필드에서 검색(해야 되는데 빠져있네?)

다시 수정해보겠다.

```java
@Query("SELECT p FROM JobPosting p " +
        "WHERE p.company.name LIKE %:keyword% " +
        "OR p.company.country LIKE %:keyword% " +
        "OR p.company.region LIKE %:keyword% " +
        "OR p.jobPosition LIKE %:keyword% " +
        "OR p.recruitmentDetails LIKE %:keyword% " +
        "OR p.technologyUsed LIKE %:keyword% " +
        "OR CAST(p.recruitmentCompensation AS string) LIKE %:keyword%")
```
로 해결 완! 마감 시간 전에 고쳐서 정말 다행이고.. 이래서 기록이 중요하구나 다시금 깨닫습니다요..

검색 결과 폼은 코드는 생략하고 아래의 실행 예시에서 확인 가능하다.

[실행 예시]
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/779b233b-31da-45af-9e0f-55e88d187cdb)

드디어 모든 기능 소개가 끝났다!! 
원래는 게시글 하나로 프로젝트를 정리하려고 했지만 너무나 욕심이었고.. to be continued....