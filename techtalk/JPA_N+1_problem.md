# JPA - N+1 문제

두 엔티티가 양방향으로 연관 관계를 맺고 있는 상황에서 상대방 엔티티를 로드했을 때 끊임없이 참조 중인 엔티티를 로드해서 발생하는 문제

## 어떤 상황에서 발생하지?

<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/95b226de-919d-4886-b655-7a5cbd37f777/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=ASIAT73L2G45OYA7JQWI%2F20191027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20191027T040524Z&X-Amz-Expires=86400&X-Amz-Security-Token=AgoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJGMEQCIB8k9G%2FJp%2F9JSBG1vdxxLDuga9qflGTy2sR9zFxsLUzgAiA3gJkR5UaBTDSV0jtD2yiXoNrXik%2FnMftP7%2B6Rc5Z6VSr8AwhrEAAaDDI3NDU2NzE0OTM3MCIMrHHkHir%2FDhrTIIreKtkDraUn46F0BESjx4uQ%2FNJKfRIV3qSiTG7acmxo612tm%2F367uNwZkOyy57rN8LMnvhvGppMMl7piPlyihn0K35nUTvjwIav2h8HS7kGyekVFmerwGwlTtwIvxLoDvyRLSSREkXvTyFO%2BT4SSeHwr%2BrqfYj7YDQdn8ZVNafdUqEVu%2BixeqoYpOSdS%2BkkV4PRRBlbUgpF3n4Jb%2BAGlysALVtvEmd%2BwPoPEhj7wNKA5L5fr0tMrXcHIz%2FnmYnWL8ONoSrjJmNvhxbztCiLcG1fw6qVzG%2F0AAydAKQ2Sm55yfTNPxth1wj6qx6uJA72maAvg3wwzd0sBNEnZ%2BX%2Brakfg6pHhdECPgYJJA6O2mKLWTQiTHUF8b7SmpLSgUEx0vvGRGCASMt%2BqoBTgoQj9nmLBl4WYCpIo03%2BeSMEHzczXJFxXivmYAE0ue6Yz3JUZ%2BbJRty7BBlyw9qVRWyYwS9qpSp0SHP%2FBEDL0exr%2FIwGo%2FL5SnXnFHZ95%2FoPFLuYBFexi10kd9Rc9LIPo6u9ieKqkmeV8nOFWukdQgyNLYiWgDs4Grgrcjuc8y380s8dbwDi1nUUYp7Ept35LafALd%2Fnyu0DxdxY8hk%2B9QCJbl335rbenOlBOT4%2FueKfc5Yw%2F%2FDT7QU6tQHGzBhXtzlBOaFuzeFHPm4l%2FCw909K2iNxgw%2FXIcgkuEAaOxQ0CVu9fpijjfsBZiMc%2BhEa3lco%2F3beKA4dbECH6DKn5vNmVDuk3hmZ8TnM%2BZ32qBjzyYF%2FguTNrY0ONvcsYhb9%2FSkB202IVm4fC19JvCHqH54Ya4AmVwkRLccNJXLIfQVFLVrldOijzSn0fXZOKx7SkBQk8brMNTc%2BCyS3ZzVgMP4gR01MrvyefifDGXglAOneA&X-Amz-Signature=19bd648a26dc66dbca0174b9095c477b0a575cd6d0d600ec535152e6ebc5a5e7&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22"></img>

- 두 엔티티의 연관관계가 **양방향**으로 설정돼 있음
- 회원.orders()를 **즉시 로딩**으로 설정
- 특정 회원 하나를 `em.find()`로 조회하면 주문에 대한 정보도 함께 조회

<br>
<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2c92b1ff-8505-4e8d-a6b3-6a3832d13391/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=ASIAT73L2G45JOXUT2MB%2F20191027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20191027T040512Z&X-Amz-Expires=86400&X-Amz-Security-Token=AgoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJHMEUCIQCNIrClE7lXh%2BW4%2BwAWLJKhbEuGIdmfTI40hDQJHU5ADgIgJCbqnhaMHQMOOaqe%2FVkB9nqQ1LqySya6pmwuM%2FOaDbsq%2FAMIaxAAGgwyNzQ1NjcxNDkzNzAiDKR9OK7jaebdGwfyASrZA69O5i4v7yaZYEz1jpBVQlg8hWxzIym%2B4VcyC1zaEWDCbzI%2FtegWApRbDpZ0tfe6oYo0SeThIqFIqMbis8o%2B4taEFtRwJoeK0h3NLXWq0fVRprc4HE2ia9hDHT3eZB1UdCbbw0kzm%2BOHCkACwkBj%2FFGOQCa4o8LFivkqUzDzgMk1wQk%2BOTNtiBhB5NYWTxxAimAmrfKcWsF6iif%2B6OLgwFK947Cbrzbzs3MPR4HTabrIUu8XQP%2FXsiGHB21a%2FJPhTsdSsaxRV4ag7xdHl6s0sgZ8OCeq62a6Ur9YKZkFQ3UIKHhEKSmTBums7DU%2FQ4SKk5vAs3aOSPEKeY2SCrogSCsrL68JrIXAlYBBOE7ytduiXbBdBKCTF4JkH8hCibB0fXM1882YpcuMXxMJ1hZehpw3UGjdgtn2zseXRa8E8hRkq0riFYymNzQd2Zj8eb9OqulUfT0gETfQQUsc8DhhTb7l65zaSDD8sPEI5T3%2FdKpBNogSjaNy0F7MNIrWsIi3Ut1NH9%2BVjk7C3MXJkJijSe5JbexBmV20QGlSa4ney5YmwZ9ceqbKvSXk%2FDHrUxgOWJRjyRUdOVgbssfLBRJEdZKnfqRstflM9OQ0hvdl4g%2B3%2Bpq9S7J3MGDpMN%2Fw0%2B0FOrQBIvleHBq7GQEBrPgCsts%2BWrbwWNORbhOsr7l%2Ft2y248H5sd1NO1vCejqEmrrhTAp7%2BPQqMtjsWLt2nzhdl23mw6zAouOMSyF2NE8oMCnGQElEJRpdEydXdpnr2V1sLIIIZxlu5i1tsCHBUio3q%2FivLgHSOfej3ZT26Ffod%2Bs0tjRN%2BVHB%2BoW0iRJbvQJyahYwTgnWCoQcnqpyCecDxY0Y3LyceFtVZPPqvqUCVPavSGTazk%2F8&X-Amz-Signature=0ceae5ca50aef6903ca51932d6d8b992f69f64b3ad2131724988cf52c3bd4840&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22"></img>

필요한 엔티티를 사슬처럼 계속 갖고 와

### 지연 로딩으로 바꾼다면

비즈니스 로직에서 주문 컬렉션을 실제로 사용할 때 발생하게 되겠지

## 어떻게 해결하지?

- fetch join 사용

    : 페치 조인은 SQL 조인을 사용해서 연관된 엔티티를 함께 조회하기 때문에 N+1 문제가 발생하지 않음

    참고하자 → [https://jojoldu.tistory.com/165](https://jojoldu.tistory.com/165)

- 하이버네이트가 제공하는 @BatchSize 사용

    : 연관된 엔티티를 조회할 때 지정한 size만큼 SQL의 IN 절을 사용해서 조회

        @org.hibernate.annotation.BatchSize(size = 5)
        @OneToMany(mappedBy = "member", fetch = FetchType.Eager)
        private List<Order> orders = new ArrayList<Order>();

- 나의 경우, @JsonManagedReference 애너테이션을 사용

    객체의 상위/하위 관계를 처리 및 명시하고 무한 순환 참조 에러 해결
