# Setting up the harbor registery
Consider the following.

- the user is your email and the password is the access token from the epd
- after you create the images.txt file copy the conten of each image file to it and run it alone
- to run the script without having to stay next to the pc .. run the following `nohup ./image_sync_to_private_registry.sh > result 2>&1 &`.
    - This will save the results of the script running in a file called **result** in the same directory.
    - If you want to watch the fetching results live then you can run `tail -f result` after running the previous command
    - This has the benefit of protecting against connection loss to the server, because `nohup` runs in it's own terminal, while being able to watch the results as they are fetched.
- Before running the script make sure that both files have the correct permissions to run the installation scripts by running the following command on each script file in the folder
    - `chmod +x <script name>`
- To download the image files just copy the name of the image file from the browser and do `vim <file name>` then open the link in the browser which should take you to a blank screen with images. copy all images and paste them in the open editor and then do `:wq` after that run `dos2unix <file name>` to resolve any **EOF** issues
