# How to automate API with Postman

## Assumption

You have basic knowledge about Postman.
You have development skill on javascript or any of the others programming language but not require.
You know what is an Restful Api and how to test that.

## What is a Restful Api

Restful Api is one of the most common think you should know when you are a quality engineer (QE). In addition, QE mean manual tester or  automation tester. If you do not have any about "What is the Restful Api" please visit <https://restfulapi.net>.

Basically, Restful Api contains some informations consist of Url, method, header, param and body. All of that information will be sent to an application. Then, the application will response informations that contain status code and response body.

## How to test a Restful Api

Restful Api basically is about sending request to an application and receiving the response. Therefore, testing a Restful Api is just verify that when I send the request with specific request, the system will response back the information as I expected. For example, we have to test a Calculation Restful Api that will receive the request and response calculated number. For more details, the request consists of method, Url and 2 params. 2 params are NumberA and NumberB that are 2 number send to the system and expect the system will response the sum of that two variable (NumberC = NumberA + NumberB).

To test the Calculation Restful Api, we have to design some test case to verify is that system work correctly. There are some basic test cases we can create to test that system. Firstly, We create the valid case about input value 1 for NumberA and 2 for NumberB so the response NumberC should be 3 and the status code is 200. Secondly, we create the test case that the numberA is 1 and the NumberB is "Two" so the response should show error message "The numberB is not a number." and the status code is 400. Finally, We need to check the method of request that the system will response error message "Not found " and status code is 404 if we input the wrong request's method.

In conclusion, Testing Restful Api is about create the test cases that ideally cover all of aspects of that Api.

## How to test an Api test case manually by Postman

To be clear, there are many type of Api but this document just focus on Restful Api so I will call the Restful Api with name is Api.

The previous section, I give you some test cases to test an Api such as happy case, validation case, but there is just one way to implement an Api test case. Therefore, I will go to detail in just an Api test cases. The Api test case that I mention in this section consist of:

- Api test case name: Get User information successfully
- Api request information:
  - Name: Get user by userId
  - Method: Get
  - Url: <https://5f9cbdef6dc8300016d3139b.mockapi.io/w33/users/{{UserId}}> (An mock Api from MockApi.com)
- Api response information:
  - Status code: 200
  - Response body: contains UserId

Note: UserId is a number that is Id of user. So, the UserId must be an Id of a valid user.

To manual test this test case, we will following this steps:

- Open Postman
- Create new Collection (Example: **W33Automate**)
- Create new folder inside **W33Automate** to contain all test cases for one Api testing type (Example: **Functional Testing**).
- Create new folder inside **Functional Testing** to contain all test cases for one Api site (Example: **MockApi**).
- Create new folder inside MockApi to contain test case **GetUserByIdSuccessfully**.
- Create new request inside **GetUserByIdSuccessfully** to execute the get-user-by-userid-Api. The request consists of:
  - Method: Get
  - Url: <https://5f9cbdef6dc8300016d3139b.mockapi.io/w33/users/50> (Suppose that 50 is an valid UserId).
- Execute the request **GetUserByIdSuccessfully** by Send button.
- Verify manually by view result on Postman APi result

There are all the steps I will following to test an Api test case and I work perfectly.

## How to Automate your manual test script

Suppose that you have already implemented an **GetUserByIdSuccessfully** test case and you feel that verify the result by your own eyes is so painful. I will guy you to do a small step to make the Postman tools verify the result automatically. To verify the status code and the response body, you open the Test tab and write some Javascript script (**Don't worry. It is so easy!**). We will have 2 small script to verify 2 acceptance criterial into Test tab.

- Verify the status code is 200.

```js
pm.test("Status code is 200", function () {
  pm.response.to.have.status(200);
});
```

- Verify that the response body contain the UserId.

```js
var UserId = pm.collectionVariables.get("UserId");
pm.test("Your test name", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.id).to.eql(UserId);
});
```

Note: There are a lot of functions you can use that are explain [here](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/)

