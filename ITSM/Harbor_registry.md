# Setting up the harbor registery
Consider the following.

- the user is your email and the password is the access token from the epd
- after you create the all_images.txt file copy the conten of each image file to it and run it alone
- to run the script without having to stay next to the pc .. run the following `nohup ./push_to_custom_repo.sh > result 2>&1 &`.
    - This will save the results of the script running in a file called **result** in the same directory.