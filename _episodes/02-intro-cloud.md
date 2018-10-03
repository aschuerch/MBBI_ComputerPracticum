---
title: "Logging onto Cloud"
teaching: 5
exercises: 5
questions:
- How do I connect to a cloud instance?
objectives:
- Log onto to a running instance
- Log off from a running instance
keypoints:
- You can use one set of log-in credentials for many instances
- Logging off an instance is not the same as turning off an instance
---

<script language="javascript" type="text/javascript">
function set_page_view_defaults() {
    document.getElementById('div_aws_win').style.display = 'block';
    document.getElementById('div_aws_unix').style.display = 'none';
};

function change_content_by_platform(form_control){
    if (!form_control || document.getElementById(form_control).value == 'aws_win') {
        set_page_view_defaults();
    } else if (document.getElementById(form_control).value == 'aws_unix') {
        document.getElementById('div_aws_win').style.display = 'none';
        document.getElementById('div_aws_unix').style.display = 'block';
        document.getElementById('div_hpc').style.display = 'none';
        document.getElementById('div_cyverse').style.display = 'none';
    } else {
        alert("Error: Missing platform value for 'change_content_by_platform()' script!");
    }
}

window.onload = set_page_view_defaults;
</script>

## Important Note

This lesson covers how to log into, and out of, an *already running* cloud instance.


## Background to cloud computing

To access the pre-configured workshop data, you'll need to use our log-in credentials (user name and password). A user name will be assigned to you at the workshop. In the following, always replace dcuser with your user name 
and the password with the password assigned to you.

**Log-in Credentials (case-sensitive!)**

- Username: dcuser
- Password: password

But first, you need a place to log *into*! To find the instance that's attached to that data,
you'll need something called an IP address. Your instructor should have given this to you
at the beginning of the workshop.

An IP address is essentially the numerical version of a web address like www.amazon.com

Recall that cloud computing is about choice. You can rent just a single processor on a large computer
for a small project, or you can rent hundreds of processors spread across multiple computers for
a large project. In either case, once you rent the collection of processors, the cloud provider will
present your rental to you as if it was a single computer. So, the physical computers that host your
instances don't really move, *but* every time you launch a new instance, it will have a new IP address.

So, each time you launch a new instance, the *IP address* changes, but your *Log-in Credentials* don't have to.

## Connection Protocols

We will use a protocol called Secure Shell (SSH) that, as the name implies, provides you
with a secure way to use a [shell](http://swcarpentry.github.io/shell-novice). In our case,
the shell will be running on a remote machine. This protocol is available for every
operating system, but sometimes requires additional software.

## Logging onto a cloud instance

**Please select the platform you wish to use for the exercises: <select id="id_platform" name="platformlist" onchange="change_content_by_platform('id_platform');return false;"><option value="aws_unix" id="id_aws_unix" selected> AWS_UNIX </option><option value="aws_win" id="id_aws_win" selected> AWS_Windows </option></select>**

<div id="div_aws_win" style="display:block" markdown="1">

## Exercises<br>

#### **Connecting using PC**<br>
*Prerequisites*: You must have an SSH client. There are several free options but you should have installed [[PuTTY.exe](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)] at the begining of the workshop, and we're going to continue using that.

A. Open PuTTY; In the 'Host Name (or IP address)' section paste in the IP address provided by your instructor (or the IP address of an instance you have provisioned yourself). *Keep the default selection 'SSH' and Port (22)*.

<p><img src="../fig/putty_screenshot_1.png" width="500"></p>
B. Click 'Open' and you will be presented with a security warning. Select 'Yes' to continue to connect. <br>
<p><img src="../fig/putty_screenshot_2.png" width="500"></p>

C. In the final step, you will be asked to provide a login and password. **Note:** When typing your password, it is common in Unix/Linux not see see any asterisks (e.g. ****) or moving cursors. Just continue typing.<br>
<p><img src="../fig/putty_screenshot_3.png" width="500"></p>

D. You should now be connected!

</div>




<div id="div_aws_unix" style="display:block" markdown="1">


#### **Connecting using Mac/Linux**<br>
*Prerequisites*: Mac and Linux operating systems will already have terminals installed. Simply search for 'Terminal' and/or look for the terminal icon.<br>
![terminal icon](../fig/terminal.png)<br>


A. Open the terminal and type the following command substituting 'ip_address' for the IP address your instructor will provide (or the IP address of an instance you have provisioned yourself). *Be sure to pay attention to capitalization and spaces*<br>

~~~
$ ssh dcuser@ip_address
~~~
{: .bash}

B. You will receive a security message that looks something like the message below. Type 'yes' to proceed.

~~~
The authenticity of host 'ec2-52-91-14-206.compute-1.amazonaws.com (52.91.14.206)' can't be established.
ECDSA key fingerprint is SHA256:S2mMV8mCThjJHm0sUmK2iOE5DBqs8HiJr6pL3x/XxkI.
Are you sure you want to continue connecting (yes/no)?
~~~
{: .bash}

C. In the final step, you will be asked to provide a login and password. **Note:** When typing your password, it is common in Unix/Linux not see any asterisks (e.g. ****) or moving cursors. Just continue typing.

D. You should now be connected!

</div>

## Logging off a cloud instance

Logging off your instance is a lot like logging out of your local computer: it stops any processes
that are currently running, but doesn't shut the computer off. AWS instances acrue charges whenever
they are running, *even if you are logged off*.

If you are *completely* done with your AWS instance, you will need to **terminate** it after you log off. Instructions for terminating an instance are here: [launching cloud instances on your own](http://www.datacarpentry.org/cloud-genomics/LaunchingInstances/).

To log off, use the `exit` command in the same terminal you connected with. This will close the connection, and your terminal will go back to showing your local computer:

~~~
dcuser@ip-172-31-62-209 $ exit

Amandas-MacBook-Pro-3 $
~~~
{: .output}

## Logging back in

Internet connections can be slow or unstable. If you're just browsing the internet, that means you have
reload pages, or wait for pictures to load. When you're working in cloud, that means you'll sometimes
be suddenly disconnected from your instance when you weren't expecting it. Even on the best internet
connections, your signal will occasionally drop, so it's good to know the above SSH steps, and be able
to log into AWS without looking up the instructions each time.

In the next section, we'll also show you some programs that you can use to keep your processes going
even if your connection drops. But for now, just practice logging on and off a few times.


{% include links.md %}
