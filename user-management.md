# User and Group Management

This document covers creating users, groups and OUs in **Active Directory Users and Computers** (ADUC), setting account restrictions, and using groups to control folder access. Open ADUC from **Server Manager > Tools**.

## Creating a user

1. In ADUC, right-click the target container (here `lab.local/Users`) and choose **New > User**.
2. Fill in the name and logon name. The logon name becomes `name@lab.local`, and the pre-Windows 2000 name is `LAB\name`.
3. Set an initial password and choose the password options.
4. Review the summary and click **Finish**.

![New user testlab5](../screenshots/37_ADUC_New_User_testlab5.png)

On the password page I left **User must change password at next logon** checked, and left *cannot change password*, *never expires* and *account disabled* unchecked:

![Password options](../screenshots/38_ADUC_New_User_Password_Options.png)

I followed the same process for the student accounts `userstudent1` and `userstudent2`:

![userstudent1](../screenshots/50_ADUC_New_User_userstudent1.png)
![userstudent2](../screenshots/51_ADUC_New_User_userstudent2.png)

## Account restrictions: logon hours

On the **Account** tab of the `testlab5` properties, **Logon Hours** limits when the account can sign in. I permitted logon **Monday through Tuesday, 8:00 AM to 6:00 PM** and denied all other times.

![Logon hours for testlab5](../screenshots/39_ADUC_Logon_Hours_testlab5.png)

## Deleting a user

Right-click the user and choose **Delete**. ADUC asks for confirmation before removing the account. I removed the unused `testlab` account this way.

![Delete confirmation](../screenshots/41_ADUC_Delete_User_testlab_Confirmation.png)

## Organizational Units

I created the **CallCenter** OU and then created `TestUserLab6` inside it (**Create in: lab.local/CallCenter**).

![TestUserLab6 in CallCenter](../screenshots/43_ADUC_New_User_TestUserLab6_in_CallCenter.png)
![CallCenter OU contents](../screenshots/44_ADUC_CallCenter_OU_with_TestUserLab6.png)

OUs make it possible to link Group Policy to a specific set of users. See [active-directory.md](active-directory.md).

## Groups

Groups are created with **New > Group**. All groups here use scope **Global** and type **Security**.

| Group | Purpose |
|---|---|
| `GlobalGroup1` | Practice group |
| `studentgroup1` | Students who need access to the quizzes folder |
| `allstudents` | Group for all student accounts |

![GlobalGroup1](../screenshots/40_ADUC_New_Group_GlobalGroup1.png)
![studentgroup1](../screenshots/48_ADUC_New_Group_studentgroup1.png)
![allstudents](../screenshots/49_ADUC_New_Group_allstudents.png)

### Adding members

Open the group's properties, go to the **Members** tab, click **Add**, type the user name and select **OK**. I added `userstudent1` to `studentgroup1`:

![Select Users dialog](../screenshots/52_ADUC_studentgroup1_Add_Member_Select_Users.png)
![Members tab](../screenshots/53_ADUC_studentgroup1_Members_Tab.png)

> **TODO:** add a screenshot or note if `userstudent2` was also added to `studentgroup1`, and how `allstudents` was populated.

## Giving a group access to a folder

I created `C:\it442` with two subfolders, `lecture` and `quizzes`:

![C: drive](../screenshots/46_File_Explorer_C_Drive_it442_Folder.png)
![it442 contents](../screenshots/47_File_Explorer_it442_lecture_quizzes_Folders.png)

For `quizzes` I opened **Properties > Security > Edit > Add**, entered `studentgroup1` and confirmed it with **OK**:

![Select group for permissions](../screenshots/54_quizzes_Permissions_Select_studentgroup1.png)

`studentgroup1` was given **Read & execute**, **List folder contents** and **Read**, so its members can open the quizzes but not change them.

![quizzes permissions](../screenshots/55_quizzes_Permissions_studentgroup1_Read_Execute.png)

Assigning permissions to a group instead of individual users means adding or removing a student only changes group membership, not folder permissions.
