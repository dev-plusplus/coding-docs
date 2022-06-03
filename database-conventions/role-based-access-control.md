This is a draft into how to implement different flavours of an RBAC on a database level. 

The scope of this documents only proposes how to do a proper Database model, does not covers how to perform validation on the API calls, Cloud Function and access, or frontend code.

With the purpose of simplifying the explanation, we are gonna explain the generic RBAC model for a single Role in the system, and then explain how the model can be expanded to accomodate situations such as: multiple Roles, multi tenant RBAC, permissions based RBAC and policy based RBAC.


## RBAC: 

 We start from the context where, on the system, there is a table that represents a User identity. We will call this table `User`
 
 <img width="263" alt="Screen Shot 2022-06-03 at 9 03 19 AM" src="https://user-images.githubusercontent.com/918895/171859337-859f98bd-013b-4702-90fe-a332cdbacbd5.png">

We will also be using a reference to the Tenant of the system. Tenant on this context represents the Organization where the data that the system is handling belongs to. The Tenant is usually represented on a database like an Organization or a `Company`.


### Single Role - Single Tenant

The simples scenario for an RBAC system is where there is a single Tenant on the system. On this cases the Tenant is not represent in any way on the Database.

Regarding the role, a flexible RBAC system should always support the extension of having multiple roles per use, even if the requirement explicitly indicates that a User should only have one Role.

For achieving this, we always use a separate table to indicate a User's role, as opposed to a field on the User table:

- A separate table for indicating user roles leaves always space for grow into a multi role environment
- A separate table, gives the space to indicate different attributes to the Role such as, when or by who this role was assigned, or possible attributes related to the Role relation

To solve this scenario, we will create a new table called `UserRole` where we indicate a foreign key to the user table, and a field to indicate the Role name. Other use cases explained after will extend this Role name to a table, permission groups, policies, etc. But for now, we will just start with the role name.

<img width="675" alt="Screen Shot 2022-06-03 at 9 10 54 AM" src="https://user-images.githubusercontent.com/918895/171860574-40f51423-53d0-4d74-8ae5-e4748aa8bc05.png">
