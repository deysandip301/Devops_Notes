## Q1. Pulling an Image

```bash
docker pull nginx:1.23.4 > answer.txt

# Verify the image is downloaded
docker images

# Verify the logs
cat answer.txt
```

- `>` : Redirects output to a file (overwrites)
- `>>` : Appends output to a file

### Sample Terminal Logs
```
TTY="/dev/pt"
$ docker pull nginx:1.23.4
bash-5.1$ docker pull nginx:1.23.4
bash-5.1$
-43+00:00
> answer.txt
> /home/user/answer.txt
```
