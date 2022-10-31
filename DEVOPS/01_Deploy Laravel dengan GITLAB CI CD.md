# Proses Instal PHP,MYSQL,APACHE di BiznetGio product NeoLite
## 1. Enable SSH Public Key

Login lewat open console  
```
nano /etc/ssh/sshd_config
```

Cari baris terakhir, ubah isian di bagian <code>PasswordAuthentication </code> dari yang sebelumnya berisi <b>no</b> ke <b>yes</b>  

```
PasswordAuthentication yes
```

Lakukan reload 
```
sudo service sshd reload
```

## 2. Install Docker

Lakukan beberapa perintah berikut ini:

```
sudo apt update
```

```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
```

```
apt-cache policy docker-ce
```

```
sudo apt install docker-ce
```

```
sudo systemctl status docker
```

```
sudo usermod -aG docker ${USER}
```

```
su - ${USER}
```

Cek dengan perintah
```
groups
```
## 3. Create SSH key Di Server untuk Deploy
Jalankan perintah berikut ini untuk:
```
sudo adduser namauser
sudo setfacl -R -m u:namauser:rwx /lokasi
```
Jalankan perintah jika ACL belum ada:
```
sudo apt install acl
```
login ke aplikasi sebagai namauser:
```
sudo namauser
```
Jalankan perintah untuk membuat pasangan private key dan public key
```
ssh-keygen -t rsa
```
copy isi dari public key ke authorized key:
```
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```
## 4. Buka project yang ada di GITLAB
Buka bagian settings > CI/CD > Variables kemudian tambahkan variabel, misalnya diberi nama SSH_PRIVATE_KEY, dan isi didapatkan dari 
```
# pastikan sudah login sebagai user yang diberi akses ke folder aplikasi
cat ~/.ssh/id_rsa
```

## 5. Buat Runner
```
docker run -d --name gitlab-runner --restart always \ -v /srv/gitlab-runner/config:/etc/gitlab-runner \ -v /var/run/docker.sock:/var/run/docker.sock \ gitlab/gitlab-runner:v14.7.0
```
Daftarkan runner
```
docker exec -it gitlab-runner gitlab-runner register
```

```
sudo usermod -aG docker namauser
```
## 6. Buka project Laravel
Tambahkan file .gitlab-ci.yml, ambil contoh di file berikut  
Masukkan beberapa variabel yang diperlukan.
Buat access token di profile gitlab untuk proses git clone tanpa perlu memasukkan password.


