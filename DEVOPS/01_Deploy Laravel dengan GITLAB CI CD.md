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
#silakan install acl jika belum punya
sudo apt install acl

sudo setfacl -R -m u:namauser:rwx /lokasi

# set permission yang perlu di folder tujuan, saat sudah ada projectnya
chmod 777 -R /lokasi/storage
chmod 777 -R /lokasi/public
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
## 4. Install PHP MySQL Apache dengan Docker & Install Lets Encrypt
```
# masuk folder, buka terminal di lokasi folder
git clone https://github.com/dirumahrafif/docker-apache-php8-mysql.git .
docker-compose up -d
```
Proses install letsencrypt
```
# masuk ke container webserver
docker exec -it webserver bash
certbot --apache -d resumerafif.com -m dirumahrafif@gmail.com
certbot --apache -d resumerafif.com -d www.resumerafif.com -m dirumahrafif@gmail.com
```
## 5. Buka project yang ada di GITLAB
### Menambahkan Variabel
Buka bagian settings > CI/CD > Variables kemudian tambahkan variabel, misalnya diberi nama SSH_PRIVATE_KEY, dan isi didapatkan dari 
```
# pastikan sudah login sebagai user yang diberi akses ke folder aplikasi
cat ~/.ssh/id_rsa
```

![Tambahkan variabel](https://raw.githubusercontent.com/dirumahrafif/devlogs/main/DEVOPS/images/1.png)
## 6. Buat Runner
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
## 7. Buka project Laravel
### Tambahkan file .gitlab-ci.yml
File .gitlab-ci.yml, ambil contoh di [file berikut ini](https://gist.githubusercontent.com/dirumahrafif/71e5d2ebeada6a5be126cca638651461/raw/2fb47747a17d3a0c73de7001948e587340bf89b3/.gitlab-ci.yml)
```
VAR_DIREKTORI: "/home/rafifresume/APLIKASI/www"
VAR_GIT_URL_TANPA_HTTP: "gitlab.com/dirumahrafif/namauser.git"
VAR_CLONE_KEY: "xxx" # diambil dari halaman profile (lihat di bawah)
VAR_USER: "namauser" #user yang sudah diberi akses
VAR_IP: "xxx" #ip server
VAR_FILE_ENV: $FILE_ENV #dari point 5 di atas
VAR_FILE_HTACCESS: $FILE_HTACCESS #dari point 5 di atas
```

### Cara mendapatkan Token User
Buka halaman profile
![gambar2](https://raw.githubusercontent.com/dirumahrafif/devlogs/main/DEVOPS/images/2.png)
Masuk ke menu access token:
- masukkan <code>token name</code>
- <code>expiration date</code> dikosongkan saja
- <code>select scopes</code> saya checklist semua
- Kemudian klik tombol **Create personal access token**

![gambar3](https://raw.githubusercontent.com/dirumahrafif/devlogs/main/DEVOPS/images/3.png)
