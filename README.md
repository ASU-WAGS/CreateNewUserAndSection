# CreateNewUserAndSection

Just change the first six variables to what you need them to be

Run using the MySQL admin panel

Be sure the section name is unique. For example testuser_summer_2016 instead of testuser_section

``` SQL

SELECT @username := "testuser1";

SELECT @first_name := "test";

SELECT @last_name := "user";

SELECT @email := "testuser1@testuser.com";

SELECT @password := "5f4dcc3b5aa765d61d8327deb882cf99";

SELECT @section_name := "testusersection1";

# Add a new section
INSERT INTO `wags`.`section` (`id`, `name`, `administrator`, `logicalExercises`, `added`, `updated`) VALUES (NULL, @section_name, '1', NULL, '1', '1');

# Get the section number from the new section
SELECT @section_number := (SELECT `id` FROM `section` WHERE name = @section_name);

# Add User
INSERT INTO `wags`.`user` (`id`, `username`, `firstName`, `lastName`, `email`, `password`, `added`, `updated`, `lastLogin`, `admin`, `section`) VALUES (NULL, @username, @first_name, @last_name, @email, @password, '1', '1', '1', '1', @section_number);

# Get user id number
SELECT @user_id_number := (SELECT `id` FROM `user` WHERE `username` = @username);

# Update admin on section that was created
UPDATE `section` SET `administrator`= @user_id_number WHERE id = @section_number;

# Add magnet groups to section
INSERT INTO `SectionMP` (`section`, `magnetP`)
SELECT @section_number, `id` 
FROM `magnetProblem` WHERE `groupID` BETWEEN 2 AND 15;

# Add all problems from the above groups to section
INSERT INTO `wags`.`SectionMPG` (`id`, `section`, `magnetGroup`) VALUES (NULL, @section_number, '2'), (NULL, @section_number, '3'), (NULL, @section_number, '4'), (NULL, @section_number, '5'), (NULL, @section_number, '6'), (NULL, @section_number, '7'), (NULL, @section_number, '8'), (NULL, @section_number, '9'), (NULL, @section_number, '10'), (NULL, @section_number, '11'), (NULL, @section_number, '12'), (NULL, @section_number, '13'), (NULL, @section_number, '14'), (NULL, @section_number, '15');

```
