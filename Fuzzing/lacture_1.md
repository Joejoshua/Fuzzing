# Introduction to FFUF tool
- Fuzzing: technique that use for test how secure of the program by enter unusual or random data into the program. to see how the program will respond.

- FFUF: tool used to automatically scan hidden files and directories on websites.

- Worldlist: (directory attack) set of words, numbers, or characters that are used to experimentally access various systems.

- We gonna learn `ffuf` command from this [website.](http://ffuf.me/)

---

### Install ffuf command
```bash
sudo apt install ffuf -y
```

---

### Download Worldlists
```bash
wget http://ffuf.me/wordlist/common.txt

wget http://ffuf.me/wordlist/parameters.txt

wget http://ffuf.me/wordlist/subdomains.txt

ls
common.txt parameters.txt subdomains.txt
```

---

### Lab 1: Content Discovery - Basic
- Content Discovery: Accidentally discovered the content you were looking for

```bash
ffuf -w common.txt -u http://ffuf.me/cd/basic/FUZZ
# Found hidden files name class and development.log         
```
- `-w`: /path/to/wordlist.txt
- `-u`: URL target
- `FUZZ`: Special placeholder keyword that ffuf will replace with values ​​from the word list to create test cases.

- Check the file by using `curl` command.
```bash
curl http://ffuf.me/cd/basic/class
You Found The File!

curl http://ffuf.me/cd/basic/development.log
You Found The File!
```

---

### Lab 2: Content Discovery - Recursion
```bash
ffuf -w common.txt -recursion -u http://ffuf.me/cd/recursion/FUZZ
# Found hidden directories name /admin and /users
# And found hidden file name 96
```
- `-recursion`: Scan repeatedly in newly discovered directories. To find deep hidden directories 

- Check the file by using `curl` command.
```bash
curl http://ffuf.me/cd/recursion/admin/users/96
You Found The File!
```

---

### Content Discovery - File Extensions
- Finding a file extensions
```bash
ffuf -w common.txt -e .log -u http://ffuf.me/cd/ext/logs/FUZZ
# Found users.log file
```
- `-e`: Extension
- `.log`: Finding *.log file extensions  

- Check the file by using `curl` command.
```bash
curl http://ffuf.me/cd/ext/logs/users.log
You Found The File!
```

---

### Content Discovery - No 404 Status
```bash
ffuf -w common.txt -u http://ffuf.me/cd/not404/FUZZ -t 100
# Found alot of files
# Example: .git/HEAD   [Status: 200, Size: 669, Words: 126, Lines: 23]
```
- Because no 404 (Page Not Found) headers are sent, ffuf interprets the file as found every time it makes a request. Even if the file doesn't actually exist.

- There are suspicious "Page Cannot Be Found" pages because they have the same file size every time.
```bash
ffuf -w common.txt -u http://ffuf.me/cd/no404/FUZZ -fs 669
# Found a file name secret 

ffuf -w common.txt -u http://ffuf.me/cd/no404/FUZZ -fw 126 -ac
# Found a file name secret
```
- `-fs`: Filter HTTP response size. 
- `-fw`: Filter HTTP response word.
- `-ac`: Work on multiple requests to increase the speed of inspection

- Check the file by using `curl` command.
```bash
curl http://ffuf.me/cd/no404/secret
Controller does not exist
```

---

### Content Discovery - Param Mining
```bash
curl http://ffuf.me/cd/param/data
Required Parameter Missing

curl -i http://ffuf.me/cd/param/data
HTTP/1.1 400 Bad Request
```
- `-i`: Show HTTP topics (headers)

```bash
ffuf -w parameters.txt -u http://ffuf.me/cd/param/data?FUZZ=1 -mc 200,204,301,302,307,401,403,405,
415
# Found 1 missing parameter name debug
```
- `-mc`: Mach the status code that I want

- Check the file by using `curl` command.
```bash
curl http://ffuf.me/cd/param/data?debug=1
Required Parameter Found

curl -i http://ffuf.me/cd/param/data?debug=1
HTTP/1.1 200 OK
```

---