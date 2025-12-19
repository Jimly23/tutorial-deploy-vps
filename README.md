1. Update Repository
```
sudo apt update
```
2. Install Package Yang diperlukan
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
```
3. Tambah GPG Key
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
4. Tambah Docker repository
```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```
5. Pastikan sumber 
```
apt-cache policy docker-ce
```
6. Install Docker
```
sudo apt install docker-ce -y
```
7. Cek status Docker
```
sudo systemctl status docker
```
