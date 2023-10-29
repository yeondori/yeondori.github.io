---
title: "[프로젝트]원티드 프리온보딩 과제 수행 05 프로젝트 리팩터링"
excerpt: "원티드 프리온보딩 과제 수행 과정 기록"
categories: [Spring, Pre-onboarding]
tags: [Spring]
date: 2023-10-25
last_modified_at: 2023-10-26
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
  
  