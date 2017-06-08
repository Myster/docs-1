### Example Scenario
We have a team that should be dedicated to a specific project. They should be able to create and edit anything to do with this project (i.e. deployment process, variables, channels, etc.). To accomplish this, this guide will walk through the following steps.

1. Create a custom role
2. Create a new user
2. Create a new team to assign this role and user to

### Process

1. Let's start off by creating the custom role. You will need the following permissions to allow full control of projects. (We can limit which projects as shown in a later step).

Environment view
Environment manager
Project view
Project manager

Your custom role should look something like the following.

[image]

2. Now let's create our new user. Here, you assign this user the usual username, password and email address.
*Configuration > Users*

3. Let's create a new team to assign our new role to.

[image]

This will allow users in this team to be able to edit all projects. We can then scope this team to specific projects.

[image]

I hope this provides some good insight as to how permissions, roles and teams work together, and shows the potential to how granular these permissions can be in limiting permissions to your users.
