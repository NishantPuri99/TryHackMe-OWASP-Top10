# TryHackMe-OWASP-Top10

![Intro](Introduction_Image.png)

**[Click Here and Try It Out!](https://tryhackme.com/room/owasptop10)**

## [OWASP Top 10 - A challenge everyday for 10 days]

Learn one of the OWASP vulnerabilities every day for 10 days in a row. A new task will be revealed every day, where each task will be independent from the previous one. These challenges will cover each OWASP topic:


> My First Try at Hacking Lab Write-Ups ;)

### Day 1:

**Vulnerability:** <code> Injection </code>

**Target:** <code>http://MACHINE_IP/evilshell.php.</code>
***Simple Description: A Search bar is given, we also know that the PHP Code for the same allows command injection***

**Questions:**

![Answers](Answers_Day_1_(Blurred).png)

#### Approach for each Question: (Answers are at the end)
**Question 1:** <code> What strange textfile is in the website root directory ? </code><br>
**My Solution:**
<p>A simple <code>ls</code> command gave away the name of a textfile. 
Ideally, I should have also checked the root directory using <code>pwd</code>.</p>

**Question 2:** <code> How many non-root/non-service/non-daemon users are there ? </code><br>
**My Solution:**
<p>This seemed difficult at first, on running <code>cat /etc/passwd</code>, even though all the users were displayed, still I wasn't able to figure out much.
  I searched up online and then used <code> cut -d: -f1 /etc/passwd </code> to get only the usernames. Comparing this output with a similar output on my own
terminal led me to realise that there are no such non-special users.</p>


**Question 3:** <code> What user is this app running as ? </code><br>
**My Solution:**
<p>This was easy, a simple <code>whoami</code> did the task.</p>

**Question 4:** <code>What is the user's shell set as ? </code><br>
**My Solution:**
<p>This was the trickiest in my opinion. I used this amazing guide on the forums to figure it out. <a href ="https://medium.com/@tirthesh.pawar/owasp-top-10-day-1-injection-e1d5b15b1baf">Link to the Article</a>. On deeper analysis of the <code>cat /etc/passwd</code> result. We find the answer. I owe this answer fully to this article. I realised that I needed to know what <code>cat /etc/passwd</code> actually gave.</p>

**Question 5:** <code> What version of Ubuntu is running ? </code><br>
**My Solution:**
<p>This again was pretty easy. <code>lsb_release -a</code> did the job.</p>

**Question 6:** <code> Print out the MOTD. What favorite beverage is shown ? </code><br>
**My Solution:**
<p>I tried a pretty amateur apporach at this. On opening the contents of the file that we found in *Question 1*, I thought I'd try out the same as the answer and it worked!
  Yet actually, (again had to use this <a href ="https://medium.com/@tirthesh.pawar/owasp-top-10-day-1-injection-e1d5b15b1baf">article</a>) the "message-of-the-day" file had been changed to "00-header" as mentioned in the *Hint*.Thus, using <code>cat /etc/update-motd.d/00-header</code>, the answer was finally revealed.</p>

#### Answers: (CAUTION!: If you are also trying this machine, I'd suggest you to maximise your own effort, and then only come and seek the answers. Thanks.)
**Q1:** <code>drpepper.txt</code>
**Q2:** <code>0</code>
**Q3:** <code>www-data</code>
**Q4:** <code>/usr/sbin/nologin</code>
**Q5:** <code>18.04.4</code>
**Q6:** <code>Dr Pepper</code>

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Day 2:

**Vulnerability:** <code> Broken Authentication </code>

**Target:** <code>http://MACHINE_IP:8888</code>
***Simple Description: A SignIn Button and a Register Button is given on the top, 2 fields are given for Sign-Up and a new set of 3 fields is opened up on Registration***

**Questions:**

![Answers](Answers_Day_2_(Blurred).png)

#### Approach for each Question: (Answers are at the end)
**Question 1:** <code> 	What is the flag that you found in darren's account ? </code><br>
**My Solution:**
<p>We are given that there is an account named <code>darren</code> which contains a <i>flag</i>. To access this account, if we try something like <code>darren </code> (Notice the space at the end), or even <code>   darren</code> (3 spaces in the front), for <b>REGISTERING</b> a new account and then we try Logging in with this account. Then we are able to access the account details, in this case, the <i>flag</i> from the actual <i>darren</i> account.</p>

**Question 2:** <code> Now try to do the same trick and see if you can login as arthur. </code><br>
**Not Solution Based, only apply the above method again.**

**Question 3:** <code> What is the flag that you found in arthur's account ? </code><br>
**My Solution:**
<p>By trying the same method as in Darren's account, we are able to reach the flag in this one too! <br>What's important though, is going to the next level. Thus, I tried out various different types of alternative inputs like <code>arthur.</code> <code>art hur</code> <code>_arthur</code> <code>"arthur"</code>. <br>Well, none of those actually work and thus I realised that only <i>blank spaces</i> can be used to check Broken Authentication successfully.</p>

#### Answers: (CAUTION!: If you are also trying this machine, I'd suggest you to maximise your own effort, and then only come and seek the answers. Thanks.)
**Q1:** <code>fe86079416a21a3c99937fea8874b667</code>
**Q2:** <code>No Answer Required</code>
**Q3:** <code>d9ac0f7db4fda460ac3edeb75d75e16e</code>

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Day 3:

**Vulnerability:** <code> Sensitive Data Exposure </code>

**Target:** <code>http://MACHINE_IP</code>
***Simple Description: A wesbites is given. We need to access the SQLite database and find crucial leaked information***

**Questions:**

![Answers](Answers_Day_3_(Blurred).png)

#### Approach for each Question: (Answers are at the end)
**Question 1:** <code> 	What is the name of the mentioned directory ? </code><br>
**My Solution:**
<p>I used the hint for this. But after that it became pretty clear. An important point to be noted is that <i>View Page Source</i> and more over looking it at very closely is a really necessary skill that all budding Ethical Hackers and Security Researchers need to understand!</p>

**Question 2:** <code> Navigate to the directory you found in question one. What file stands out as being likely to contain sensitive data ? </code><br>
**My Solution:**
<p>This was pretty simple. When sensitive data is directly under the root directory, then you can directly see the "database file" that we need to access.</p>

**Question 3:** <code> Use the supporting material to access the sensitive data. What is the password hash of the admin user ? </code><br>
**My Solution:**
<p>This requires understanding the support material about SQLite Databases. The basics are as follows: <br>
<ol>
  <li>Run <code>file <filename></code> in the <b>terminal</b>. This gives you the "File Type" and "Version" of the same file-type.</li>
  <li>Since it is an SQLite DB, we use <code>sqlite3 <filename></code> to access the tables under it.</li>
  <li>A really important command to be used is <code>.help</code>. Infact, we should use this anywhere and everywhere, if we're unfamiliar to the specific command.</li>
</ol>
After this, we just need to run some of the commands mentioned in the Support Material related to SQL Queries.
</p>

**Question 4:** <code> Crack the hash. What is the admin's plaintext password ? </code><br>
**My Solution:**
<p><a href="https://crackstation.net/">Crack-Station</a> is the "go-to" place for Cracking Hashes. What's more interesting is that you can download the 15GB wordlist for your own use as well!</p>

**Question 5:** <code> Login as the admin. What is the flag ? </code><br>
**My Solution:**
<p>Once we have the admin access from the SQLite Database, we just need to login as admin and the flag appears right there.</p>

#### Answers: (CAUTION!: If you are also trying this machine, I'd suggest you to maximise your own effort, and then only come and seek the answers. Thanks.)
**Q1:** <code>/assets</code>
**Q2:** <code>webapp.db</code>
**Q3:** <code>6eea9b7ef19179a06954edd0f6c05ceb</code>
**Q4:** <code>qwertyuiop</code>
**Q5:** <code>THM{Yzc2YjdkMjE5N2VjMzNhOTE3NjdiMjdl}</code>
<br>
**Bonus:**<br>
This was really fun to try out. Here goes the description for the same:<br>
<i>To spice things up a bit, in addition to the usual daily prize draw this box also harbours a special prize: a voucher for a one month subscription to TryHackMe. There may or may not be another hint hidden on the box, should you need it, but for the time being here's a starting point: boxes are boring, escape 'em at every opportunity.</i>
<br>
<p>I tried various things here, <code>ssh</code>, <code>nmap</code>, <code>metasploit</code>, but unfortunately, I failed to get through or even find the answer. I wasn't disheartened though. This bonus question has been an amazing learning experience 😊</p>

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
