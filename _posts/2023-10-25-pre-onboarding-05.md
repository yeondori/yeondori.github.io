---
title: "[프로젝트]원티드 프리온보딩 과제 수행 05 프로젝트 리팩터링"
excerpt: "원티드 프리온보딩 과제 수행 과정 기록"
categories: [Spring, Pre-onboarding]
tags: [Spring]
date: 2023-10-25
last_modified_at: 2023-10-29
render_with_liquid: false
---
# [프리온보딩 인턴십 수행과제](https://bow-hair-db3.notion.site/1850bca26fda4e0ca1410df270c03409)

프로젝트는 [🚀여기🚀](https://github.com/yeondori/wanted-pre-onboarding-backend)에서 확인할 수 있다.

-------------
# 0. README.md 수정

- 데이터 모델링 구조도 변경
  ![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/f1c10122-0b03-4e16-afe1-c911a4e3611c)
  [ERD 관련 설명](https://inpa.tistory.com/entry/DB-%F0%9F%93%9A-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%AA%A8%EB%8D%B8%EB%A7%81-1N-%EA%B4%80%EA%B3%84-%F0%9F%93%88-ERD-%EB%8B%A4%EC%9D%B4%EC%96%B4%EA%B7%B8%EB%9E%A8#erd_entity_relationship_diagram_%EA%B7%B8%EB%A6%AC%EA%B8%B0)은 여기를 정독하면 좋을 것 같다.


- 필드명 명세 파트 추가
  ![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/7e661b6c-6693-4c68-80e0-2dd3d3c94718)


# 1. 필드명 변경 

- 불필요한 중복 제거 
    ```java
    private String jobPosition;
    private Long recruitmentCompensation;
    private String recruitmentDetails;
    private String technologyUsed;
    ```
    -> 이미 JobPosting 엔티티에서도 채용이라는 의미가 있다. 중복되는 '채용'과 관련된 단어를 삭제하고, 가독성 있는 단어로 대체한다. 
    ```java
    private String position;
        private Long compensation;
        private String details;
        private String techStack;
    ```

- 변수명에서 List 제거
  변수명에 List, Long, String 등의 타입을 포함하지 않도록 수정한다.
  `jobPostingList` -> `jobPostings`, `applicantList` -> `applicant`

![image](https://github.com/yeondori/wanted-pre-onboarding-backend/assets/93027942/e0e2220b-ac70-41c3-98e6-548186654279)

# 2. for문 -> Stream

우선 JobPostingDTO에 있던 for문을 Stream을 이용해 변경해주었다.

  ```java
  private List<JobPostingDTO> mapToJobPostingDTOList(List<JobPosting> jobPostings) {
  List<JobPostingDTO> jobPostingDTOs = new ArrayList<>();
  
          for (JobPosting jobPosting : jobPostings) {
              JobPostingDTO dto = new JobPostingDTO();
              dto.setId(jobPosting.getId());
              dto.setCompanyName(jobPosting.getCompany().getName());
              dto.setCompanyCountry(jobPosting.getCompany().getCountry());
              dto.setCompanyRegion(jobPosting.getCompany().getRegion());
              dto.setJobPosition(jobPosting.getPosition());
              dto.setRecruitmentCompensation(jobPosting.getCompensation());
              dto.setTechnologyUsed(jobPosting.getTechStack());
  
              jobPostingDTOs.add(dto);
          }
          
          return jobPostingDTOs;
      }
  ```
  위의 기존 코드를 아래와 같이 수정해주었다. 그래도 보기 좋진 않군.... 
  ```java
  private List<JobPostingDTO> mapToJobPostingDTOList(List<JobPosting> jobPostings) {
  
        return jobPostings.stream()
                .map(jobPosting -> {
                    JobPostingDTO dto = new JobPostingDTO();
                    dto.setId(jobPosting.getId());
                    dto.setCompanyName(jobPosting.getCompany().getName());
                    dto.setCompanyCountry(jobPosting.getCompany().getCountry());
                    dto.setCompanyRegion(jobPosting.getCompany().getRegion());
                    dto.setJobPosition(jobPosting.getPosition());
                    dto.setRecruitmentCompensation(jobPosting.getCompensation());
                    dto.setTechnologyUsed(jobPosting.getTechStack());
                    return dto;
                })
                .collect(Collectors.toList());
  }
  ```
# 3. 메서드 위치 조정 및 수정
- getJobPostingIDList() 메서드 위치 조정 및 수정 

  getJobPostingIDList()는 Company 내부에 정의되어 있는 메서드였는데 기능상 레퍼지토리에 있는 것이 적절할 것 같다는 피드백을 들어 위치를 이동시켰다. 
    ```java
    public List<Long> getJobPostingIdList() {
        List<Long> idList = new ArrayList<>();
        for (JobPosting jobPosting : jobPostings) {
            idList.add(jobPosting.getId());
        }
        return idList;
    }
    ```
  그러나 레퍼지토리는 현재 JpaRepository를 상속받는 interface여서 body를 가질 수 없었다. 
  따라서 @Query로 member id리스트를 가져올 수 있는 방법으로 방식을 수정해주었다.
  ```java
  @Repository
  public interface CompanyRepository extends JpaRepository<Company, Long> {
  
      @Query("SELECT p.id FROM JobPosting p where p.company.id = :companyId")
      public List<Long> getJobPostingIdList(Long companyId);
  }
  ```
  그러나 다음과 같은 에러가 발생했고..
  ```
  2023-10-29T22:30:06.369+09:00 ERROR 20824 --- [nio-8081-exec-3] o.a.c.c.C.[.[.[/].[dispatcherServlet]    : Servlet.service() for servlet [dispatcherServlet] in context with path [] threw exception [Request processing failed: org.springframework.dao.InvalidDataAccessApiUsageException: For queries with named parameters you need to provide names for method parameters; Use @Param for query method parameters, or when on Java 8+ use the javac flag -parameters] with root cause

  java.lang.IllegalStateException: For queries with named parameters you need to provide names for method parameters; Use @Param for query method parameters, or when on Java 8+ use the javac flag -parameters
  ```
  메서드에 @Param을 이용해 파라미터를 설정해주었더니 해결되었다.
  `public List<Long> getJobPostingIdList(@Param("companyId") Long companyId);` 

다음으로, postController에 있는 applyForJob의 내용을 수정해주었다.

```java
  // 채용공고 지원
  public static final int SUCCESS = 1;
  public static final int MEMBER_NOT_FOUND = 2;
  public static final int POST_NOT_FOUND = 3;
  public static final int ALREADY_APPLIED = 4;

  @PostMapping("/{id}/apply")
  public String applyForJob(@PathVariable Long id, @RequestParam String memberId, Model model) {
  Long applicantId = Long.parseLong(memberId);
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

컨트롤러보다는 서비스 측면에서 지원을 다루는 게 바람직하다는 피드백을 받아 이를 서비스로 옮겨주었다.

```java
public ApplyStatus apply(Long postId, Long memberId) {

    Optional<Member> member = memberRepository.findById(memberId);
    Optional<JobPosting> selectedPost = postRepository.findById(postId);

    if (member.isEmpty()) return MEMBER_NOT_FOUND;
    if (selectedPost.isEmpty()) return POST_NOT_FOUND;
    if (member.get().getJobPosting() != null)return ALREADY_APPLIED;

    member.get().setJobPosting(selectedPost.get());
    memberRepository.save(member.get());
    postRepository.save(selectedPost.get());

    return SUCCESS;
}
```

status는 ApplyStatus라는 별도의 enum을 추가해주었고 기존 코드보다 훨씬 간략하게 작성했다.
서비스단에서 지원 자격이 있는 멤버가 적절한 지원 공고에 지원한 경우에 이를 저장하고, 모든 경우의 상태코드를 넘겨주는 작업까지 수행한다.

컨트롤러에서는 다음과 같이 채용 공고와 멤버의 id, 지원 상태를 html에 넘겨주게 된다.
```
// 채용공고 지원
@PostMapping("/{postId}/apply")
public String applyForJob(@PathVariable Long postId, @RequestParam String memberId, Model model) {
model.addAttribute("jobPostingId", postId);
model.addAttribute("memberId", memberId);
model.addAttribute("applyStatus", postService.apply(postId, Long.parseLong(memberId)));
return "jobpostings/applyResult";
}
```

4. URI 변수명 변경

기존의 post, company 컨트롤러에서는 각각의 id를 "postId", "companyId" 등으로 구분하지 않고 "id"로 통일해두었다.
이는 충분히 혼란을 줄 수 있어 다음과 같이 변수명을 수정해주었다.

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/cabc6807-2640-408e-987c-986f01e4bca5)

# 소감

드디어 대략적인 리팩터링 작업이 끝났지만 사실 아직도 만족스럽지 않은 부분이 많다.
특히, 프로젝트를 진행할 때까지만 해도 서비스와 컨트롤러를 무조건 도메인과 같은 단위로 수행해야하는 줄 알았다.
아마 이 부분에서 비롯된 오해가 서비스와 레포지토리, 컨트롤러 간의 책임과 역할의 경계를 모호하게 만들지 않았나 싶다.
이 부분까지 완전히 수정하는 것은 무리가 있을 것 같고 책임과 역할에 대한 큰 교훈을 얻고 다음 프로젝트를 위한 타산지석으로 남겨두는 걸로...
다음으로는 간단한 테스트 코드를 작성해 볼 것이다!