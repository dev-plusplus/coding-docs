This is a draft into how to implement different flavours of an RBAC on a database level. 

The scope of this documents only proposes how to do a proper Database model, does not covers how to perform validation on the API calls, Cloud Function and access, or frontend code.

With the purpose of simplifying the explanation, we are gonna explain the generic RBAC model for a single Role in the system, and then explain how the model can be expanded to accomodate situations such as: multiple Roles, multi tenant RBAC, permissions based RBAC and policy based RBAC.


## RBAC: 

 We start from the context where, on the system, there is a table that represents a User identity. We will call this table `User`
 
 <img width="263" alt="Screen Shot 2022-06-03 at 9 03 19 AM" src="https://user-images.githubusercontent.com/918895/171859337-859f98bd-013b-4702-90fe-a332cdbacbd5.png">

We will also be using a reference to the Tenant of the system. Tenant on this context represents the Organization where the data that the system is handling belongs to. The Tenant is usually represented on a database like an Organization or a `Company`.


### Base Scenario: Simple Role - Single Tenant

The simples scenario for an RBAC system is where there is a single Tenant on the system. On this cases the Tenant is not represent in any way on the Database.

Regarding the role, a flexible RBAC system should always support the extension of having multiple roles per use, even if the requirement explicitly indicates that a User should only have one Role.

For achieving this, we always use a separate table to indicate a User's role, as opposed to a field on the User table:

- A separate table for indicating user roles leaves always space for grow into a multi role environment
- A separate table, gives the space to indicate different attributes to the Role such as, when or by who this role was assigned, or possible attributes related to the Role relation

To solve this scenario, we will create a new table called `UserRole` where we indicate a foreign key to the user table, and a field to indicate the Role name. Other use cases explained after will extend this Role name to a table, permission groups, policies, etc. But for now, we will just start with the role name.

<img width="675" alt="Screen Shot 2022-06-03 at 9 10 54 AM" src="https://user-images.githubusercontent.com/918895/171860574-40f51423-53d0-4d74-8ae5-e4748aa8bc05.png">

To enforce a Single Role scenario an `unique index` can be created with the `userId` or using the `userId` as a primary key.

### Supporting multiple Roles with a Single Tenant

On a single Tenant scenario, supporting multiple Roles can be achieved easily by simply removing the `unique index` for the `userId` field, and either agregating an `id` for the table, and allowing including multiple `userId` rows.

Optionally to make sure that we keep integrity, we can add an `unique index` with the `userId` and `name`.

<img width="724" alt="Screen Shot 2022-06-03 at 9 27 24 AM" src="https://user-images.githubusercontent.com/918895/171863477-c93b08db-837d-4a9e-8682-ee5a5356c38a.png">

### Supporting multiple Tenants

The concept of Multi Tenant consist on supporting data on our system from multiple Organizations. We can use the RBAC structure to indicate which User belongs to which Tenant, and which Roles does it have in that Tenant. 

<img width="1200" alt="Screen Shot 2022-06-03 at 9 31 10 AM" src="https://user-images.githubusercontent.com/918895/171864128-8382bbfb-4c66-40c7-94a3-d2bd894f2164.png">

Conviniently this allow a single User identity (email, phone number, etc) to have roles on multiple tenants. 

### Supporting Permissions

In more complex scenarios, Roles consists in a dynamic definition of `Permission`.  A `Permission` is the database abstraction of "something that a user can do". 

Example of permissions are: Create an Invoice, Update a User, Send and email, etc

This abstraction is represented simply with a Database table with a name or unique identifier, preferably nmemonic to communicate some sort of information with simply reading it. 

In this cases, we create a table for managing Permissions, a table for managing Roles, and a table for indicating which Permissions a Role has.

<img width="1148" alt="Screen Shot 2022-06-03 at 9 36 58 AM" src="https://user-images.githubusercontent.com/918895/171865105-dba0a030-aaf5-4948-b90c-66fdfe36538b.png">

This scenario allows the System management to define and update Roles dinamically based on which Permissions does the Role has. 

### Roles and Permissions per Tenant

A more advanced variation of the previous scenario exists, where each Tenant on the system can define custom roles to assign to the Users. In this case, we create Roles per tenant, and shift the relation between a User and a Tenant to the Roles table.

<img width="1129" alt="Screen Shot 2022-06-03 at 9 39 34 AM" src="https://user-images.githubusercontent.com/918895/171865563-1377ef18-a0dd-4176-bb9a-1c6e8b6daa48.png">

### Policies or Attirbutes

So far we have reviewed access control based on Role or Permission, the decision that the system has to take to whether allow a User to perform an action is based on whether it has a Role or not, or has a Permission or not.

 a policy extend this, usually with code.

A Policy is an advance rule that decides whether a User can perform an action or not. In the previous scenarios we simply decide whether a user can perform an action or not, based on whether he has a Permission or not, 

A Policy or rule includes this code and possibly metadata to take the decision of allowing or not the user to have 
