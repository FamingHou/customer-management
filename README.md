# Customer Management

## Overview

## Installation

### Prerequisites

* [Install npm](https://nodejs.org/en/)
* [Install Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Install Visual Studio Code](https://code.visualstudio.com/download)
* [Install Eclipse](https://www.eclipse.org/downloads/)
* [Install Angular CLI](https://angular.io/guide/quickstart)

### Frontend

* [Repository](https://github.com/FamingHou/customer-management-frontend)

* Download source code
```console
C:\project> git clone https://github.com/FamingHou/customer-management-frontend.git
C:\project> cd customer-management-frontend
C:\project\customer-management-frontend> npm install
```
* Start up
```console
C:\project\customer-management-frontend>ng serve
```
### Backend

* [Repository](https://github.com/FamingHou/customer-management-springboot)

1. Download source code
```console
C:\project> git clone https://github.com/FamingHou/customer-management-springboot
```
2. Import it as a maven existing project in Eclipse
3. Run the class *CustomerManagementApplication* as a Java application in Eclipse

## Frameworks

### Frontend

Framework | Version
------------- | -------------
Node.js | 10.13.0
Angular | 7.3.6
Typescript | 3.1.6

### Backend

Framework | Version
-------------------------- | --------------------------
Spring boot | 2.1.3
mybatis-spring-boot-starter | 1.3.0
spring-boot-starter-web
spring-boot-devtools
MyBatis | 3.5.1

## Restful APIs

Feature | Request URL | HTTP Method	 | Request parameter | Request body | Response status
---|---|---|---|---|---|
Create a customer | api/v1/customers | POST | | Customer | 201
Find all customers | api/v1/customers | GET | SortCriteria {sortColumn, sortDirection} | | 200
Find filtered customers | api/v1/customers/conditions | GET | Customer, SortCriteria || 200
Update a customer | api/v1/customers | PUT | | Customer | 200
Updata status of a customer | api/v1/customers/updatestatus | POST | | Customer | 200
Delete a customer | api/v1/customers/{id} | DELETE | | Id | 200

## Case study

### Create a customer

1. Do post from client

![create a customer](images/create-a-customer.png)

2. Data received by controller on the server side

![data received](images/request-body-received-by-controller.png)

3. Interface *CustomerService*

```java
public interface CustomerService {
  ...
  void insert(Customer customer) throws ValidationException;
  ...
}
```

4. Class *CustomerServiceImpl*

```java
@Service
public class CustomerServiceImpl implements CustomerService {
  ...
  @Override
  public void insert(Customer customer) throws ValidationException {
    Customer found = customerMapper.findByEmailId(customer);
    if (found != null) {
      throw new ValidationException("This email address has already been registered.");
    }
    customer.setCreatedTime(System.currentTimeMillis()); // Unix Timestamp in milliseconds
    customerMapper.insert(customer);
  }
  ...
}   
```

5. Interface *CustomerMapper*

```java
@Mapper
public interface CustomerMapper {
  ...
  void insert(Customer customer);
  ...
}
```

6. *CustomerMapper.xml*

```xml
<mapper namespace="com.example.customermanagement.mapper.CustomerMapper">
  ...
  <insert id="insert">
    insert into customer (first_name, last_name, email_address, status, created_time)
    values (#{firstName}, #{lastName}, #{emailId}, #{status}, #{createdTime})
  </insert>
  ...
</mapper>  
```
