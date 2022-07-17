### Step 2 : Setup a validator and sync it to the actual state of the network
____

* __Before installation, let's check if our equipment is suitable:__
```
lscpu | grep -P '(?=.*avx )(?=.*sse4.2 )(?=.*cx16 )(?=.*popcnt )' > /dev/null \
  && echo "Supported" \
  || echo "Not supported"
  ```
  > In my case everything is fine, so let's move on
  >> ![V1](https://user-images.githubusercontent.com/101806416/179396253-2e2d966b-174f-45e9-af74-c99c7238d9f6.png)

* __First, let's update linux__
```
sudo apt update && sudo apt upgrade -y
```

* __Now Install developer tools, Node.js, and npm__
```
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -  
sudo apt install build-essential nodejs
PATH="$PATH"
```

* __Let's check the version Node.js__
``` 
node -v
```
> ![v2](https://user-images.githubusercontent.com/101806416/179396939-271d69c7-59f0-4fd0-8fd7-8c03c8a1403c.png)


* __Let's check the version Npm__
``` 
npm -v
```
> ![v3](https://user-images.githubusercontent.com/101806416/179396940-3bffcb6e-3a20-4a64-b2bf-e08cbbfc4d30.png)
