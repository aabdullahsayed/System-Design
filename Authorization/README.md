# Authorization

![Authorization](./author.svg)

---

## How applications manage permissions 
- RBAC (Role-Based Access Control)
- ABAC (Attribute Based Access Control)
- ACL (Access Control List)
+ How OAuth2 and JWTs help enforce those rules 

###  RBAC (Role-Based Access Control)

![RBAC](./Diagrams/rbac.svg)


### ABAC (Attribute Based Access Control)

Three pillars:

- User Attributes (Who - Department, Job title, Age)
- Environment Attributes (Context - Time of day, IP Address, Device and Network Type)
- Resource Attributes (What - Document Classification)



"Admins can view Payroll," an ABAC policy looks like this:

ALLOW ACCESS IF:

```
(User.Department == "HR") AND
(Resource.Classification == "Confidential") AND
(Environment.Network == "Company_VPN") AND
(Environment.Time == "09:00 - 17:00")
```


![ABAC](./Diagrams/abac.svg)


### ACL (Access Control List)

- If your name is on the list, you get the permissions written to your name
- If your name is not on the list, you are blocked

![ACL](./Diagrams/acl.svg)

## OAuth2: Delgated Authorization

![OAuth2](./Diagrams/oauth2.svg)




## Token Based Authorization
![Token](./Diagrams/token.svg)




Tokens are just the mechanism. They don't make decisions.


ACL, RBAC, and ABAC are the internal rules that decide what you are allowed to do, while OAuth2 and JWT are the secure transport mechanisms used to grant and prove those permissions across the internet.