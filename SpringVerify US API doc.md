# Welcome!

Hello and welcome to SpringVerify.

This is the API documentation section of SpringVerify for customers and users located in the USA. It is divided into three sections for ease of navigation:
1. Introductions - _where entities, environments and authentication protocals are detailed._
2. Company API flow - _where functions available to users logged in as company admins are detailed._
3. User API flow - _where functions available to users logged in as employees are detailed._

## Introduction

SpringVerify currently has two environments -- development and production. Both the environments have independent databases. While in development, it is mandatory to use the base URL of the development environment in --- ?

The language bindings are in cURL presently with node.js and PHP bindings in development. The API documentation is set up with a display area on the right to show examples.

### Internal objects

| User Roles | Response |
| ---------- | -------- |
| Company | An entity that has users. These users can be employees who are **performing the verification** or are **being verified**. |
| Admin | A user who is performing the verification. |
| Employee | A user whose details are being verified on the platform. |
| Ops User | A user from the SpringVerify-US team. |

---

***Question*** -- Is it feasible to interchange `user` and `admin` in the APIs?

---

### Environment URLs
 
**Development**: https://api-stage.us.springverify.com
**Acceptance**: https://api-acceptance.us.springverify.com
**Production**: https://api.us.springverify.com
 
>Currently we have two types of coupons -- "open" and "closed". Please contact info@springverify.com regarding coupon codes for your company.

### Authentication

Authenticating in SpringVerify is done on the basis of JSON Web Tokens. Once registered, a user will receive a token to be used for subsequent API requests. (`How to get this token again if needed/misplaced?` Should this question be answered here?)

>The user must replace `JWT_TOKEN` in the examples below with their personal token.
