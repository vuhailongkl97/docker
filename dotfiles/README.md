## This project used creating a environment with setup at [dotfile](https://github.com/vuhailongkl97/dotfiles) 

## How to use
1. Using Dockerfile only  
```
	docker build -t your_image_name .
```

2. Using docker compose
+ Custome mount folder in compose.yaml file  

```
	docker compose run app
	docker ps -a
	docker start <the doceker show in previous command>
	docker exec -it <the doceker show in previous command> /bin/bash
```
