# WSL Setup Tutorial for Windows Users (For C++ Semeesters)

### Estimated Time for Completion: 10 minutes

## Introduction
Welcome to the WSL Setup Tutorial! In this class we have 5 main projects that will be done over the course of the semester. Because every student will do these projects on the computer of their choice, and every operating system has it's own way of compiling code, it creates a problem for us TA's who grade it because it's difficult to create passoff scripts that can cater to every operating system. Because of this, we've chosen Linux as the standard operating system for compiling code. Mac users do not need to worry about this as much because Macs are very similar to Linux already.

In order to help you students who are windows users, this easy tutorial has been created for you to install WSL, which is a Linux subsystem and terminal designed for windows. In simpler terms, installing WSL on your windows computer will give you a Linux terminal that you can use to compile your code.

## Part 1: Installing WSL
<br>

1. Open your command prompt as an administrator
![img1](images/1.PNG)

2. Run the command: wsl --install
![img2](images/2.PNG)

3. After finishing, restart your computer and open the Microsoft store app (if anything regarding wsl pops up after restarting, that's fine. You can either follow the intructions you're given and skip to Part 2 or close them and continue this tutorial with no problem)
![img3](images/3.PNG)

4. Search "ubuntu", then click on "Get" to install (other ubuntu versions will also work)
![img4](images/4.PNG)

5. Open ubuntu; when it's ready, choose your language and create a username and password (preferably the same as your windows login in case you forget it later)
![img5](images/5.PNG)

6. After completing the set up, feel free to close it out. You should now be able to search for and open the wsl app on your windows search bar
![img6](images/6.PNG)

## Part 2: Setting up WSL for CS 236
<br>

7. Update WSL so we can install necessary tools ('g++' the compiler command and 'make' the command for using makefiles). Update by running the command 'sudo apt update', afterwhich you will need to type in your password
![img7](images/7.PNG)

8. Install g++ by running the command 'sudo apt install g++'
![img8](images/8.PNG)

9. Install make by running the command 'sudo apt install make'
![img9](images/9.PNG)

## Part 3: Setting up CLion to use WSL as the default terminal
<br>

10. Open CLion. Currently if you open the terminal from the bottom of the window, you will notice that CLion will open a command prompt for a terminal by default, although we can change each time to use the wsl terminal, lets go to settings and set the default terminal to wsl instead. Do this by clicking on the gear in the top right and selecting 'settings'
![img10](images/10.PNG)

11. On the left, find the 'Terminal' tab from within the 'Tools' drop down menu
![img11](images/11.PNG)

12. Change the shell path to wsl.exe as shown above

13. Select OK or Apply and close the settings menu. Then select terminal from the bottom of the CLion window again. It should now open up a wsl terminal by default from now on
![img12](images/12.PNG)

## Complete