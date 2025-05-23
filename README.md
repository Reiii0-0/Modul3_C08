[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/Eu-CByJh)
|    NRP     |            Name             |
| :--------: |      :---------------:      |
| 5025241131 |      Rafly Al Fahrezi       |
| 5025241135 |      Farikh Muhammad Fauzan |
| 5025241175 |      Derryell Josua Ekklesio Pandiangan         |

# Praktikum Modul 3 _(Module 3 Lab Work)_

### Laporan Resmi Praktikum Modul 3 _(Module 3 Lab Work Report)_

Di suatu pagi hari yang cerah, Budiman salah satu mahasiswa Informatika ditugaskan oleh dosennya untuk membuat suatu sistem operasi sederhana. Akan tetapi karena Budiman memiliki keterbatasan, Ia meminta tolong kepadamu untuk membantunya dalam mengerjakan tugasnya. Bantulah Budiman untuk membuat sistem operasi sederhana!

_One sunny morning, Budiman, an Informatics student, was assigned by his lecturer to create a simple operating system. However, due to Budiman's limitations, he asks for your help to assist him in completing his assignment. Help Budiman create a simple operating system!_

### Soal 1

> Sebelum membuat sistem operasi, Budiman diberitahu dosennya bahwa Ia harus melakukan beberapa tahap terlebih dahulu. Tahap-tahapan yang dimaksud adalah untuk **mempersiapkan seluruh prasyarat** dan **melakukan instalasi-instalasi** sebelum membuat sistem operasi. Lakukan seluruh tahapan prasyarat hingga [perintah ini](https://github.com/arsitektur-jaringan-komputer/Modul-Sisop/blob/master/Modul3/README-ID.md#:~:text=sudo%20apt%20install%20%2Dy%20busybox%2Dstatic) pada modul!

> _Before creating the OS, Budiman was informed by his lecturer that he must complete several steps first. The steps include **preparing all prerequisites** and **installing** before creating the OS. Complete all the prerequisite steps up to [this command](https://github.com/arsitektur-jaringan-komputer/Modul-Sisop/blob/master/Modul3/README-ID.md#:~:text=sudo%20apt%20install%20%2Dy%20busybox%2Dstatic) in the module!_

**Answer:**

- **Code:**

  ```
  sudo apt -y update
  sudo apt -y install qemu-system build-essential bison flex libelf-dev libssl-dev bc grub-common grub-pc libncurses-dev libssl-dev mtools grub-pc-bin xorriso tmux
  mkdir -p osboot
  cd osboot
  wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.1.1.tar.xz
  tar -xvf linux-6.1.1.tar.xz
  cd linux-6.1.1
  make tinyconfig
  make menuconfig
  make -j$(nproc)
  cp arch/x86/boot/bzImage ..
  ```

- **Explanation:**

  `put your answer here`

- **Screenshot:**

  `put your answer here`

### Soal 2

> Setelah seluruh prasyarat siap, Budiman siap untuk membuat sistem operasinya. Dosen meminta untuk sistem operasi Budiman harus memiliki directory **bin, dev, proc, sys, tmp,** dan **sisop**. Lagi-lagi Budiman meminta bantuanmu. Bantulah Ia dalam membuat directory tersebut!

> _Once all prerequisites are ready, Budiman is ready to create his OS. The lecturer asks that the OS should contain the directories **bin, dev, proc, sys, tmp,** and **sisop**. Help Budiman create these directories!_

**Answer:**

- **Code:**

  ```
  cd ~/osboot
  mkdir -p BudiBebanKelompok
  cd BudiBebanKelompok
  mkdir -p {bin,dev,proc,sys,tmp,sisop}
  ```

- **Explanation:**

  `put your answer here`

- **Screenshot:**

  <div align="center">
  <img src="https://github.com/user-attachments/assets/c8861744-89ba-4b8e-b85d-f9293a251a4e" width="1200" />
</div>


### Soal 3

> Budiman lupa, Ia harus membuat sistem operasi ini dengan sistem **Multi User** sesuai permintaan Dosennya. Ia meminta kembali kepadamu untuk membantunya membuat beberapa user beserta directory tiap usernya dibawah directory `home`. Buat pula password tiap user-usernya dan aplikasikan dalam sistem operasi tersebut!

> _Budiman forgot that he needs to create a **Multi User** system as requested by the lecturer. He asks your help again to create several users and their corresponding home directories under the `home` directory. Also set each user's password and apply them in the OS!_

**Format:** `user:pass`

```
root:Iniroot
Budiman:PassBudi
guest:guest
praktikan1:praktikan1
praktikan2:praktikan2
```

**Answer:**

- **Code:**

  ```
  cp -a /dev/null ~/osboot/BudiBebanKelompok/dev
  cp -a /dev/tty* ~/osboot/BudiBebanKelompok/dev
  cp -a /dev/zero ~/osboot/BudiBebanKelompok/dev
  cp -a /dev/console ~/osboot/BudiBebanKelompok/dev 
  cp /usr/bin/busybox ~/osboot/BudiBebanKelompok/bin
  cd ~/osboot/BudiBebanKelompok/bin
  ./busybox --install .
  ```

  ```
  cd ~/osboot/BudiBebanKelompok
  mkdir -p etc
  ```

  ```
  openssl passwd -1 root 
  openssl passwd -1 Budiman
  openssl passwd -1 guest
  openssl passwd -1 praktikan1
  openssl passwd -1 praktikan2
  ```

  ```
  cat > ~/osboot/BudiBebanKelompok/etc/passwd <<EOF
  root:\$1\$TKx.WnGZ\$ESh1cSd72LGZxJYFAFcOD0:0:0:root:/root:/bin/sh
  Budiman:\$1\$hjXyqCxc\$Is4q5xLrgxzvIC3Jyga02/:1000:100:Budiman:/home/Budiman:/bin/sh
  guest:\$1\$hFqXaXPq\$jjxY7HX/GEQte1SzCmZBH1:1001:100:Guest:/home/guest:/bin/sh
  praktikan1:\$1\$fE64xkWL\$npQg.rIItYCk9INlZ8mE21:1002:100:Praktikan 1:/home/praktikan1:/bin/sh
  praktikan2:\$1\$pucNMrNN\$Y5WsJetyLbCPda6O2wfqt.:1003:100:Praktikan 2:/home/praktikan2:/bin/sh
  EOF
  ```

  ```
  cat > ~/osboot/BudiBebanKelompok/init <<'EOF'
  #!/bin/sh
  
  mount -t proc none /proc
  mount -t sysfs none /sys
  mount -t devtmpfs none /dev
  
  hostname BudiBebanOS
  
  for tty in tty1 tty2 tty3 tty4 tty5
  do
      while true; do
          /bin/getty -L $tty 115200 vt100
          sleep 1
      done &
  done
  
  wait
  EOF

  chmod +x ~/osboot/BudiBebanKelompok/init

  ````

- **Explanation:**

  `put your answer here`

- **Screenshot:**

  <div align="center">
  <img src="https://github.com/user-attachments/assets/c9b6b269-e2d6-4ffc-a266-cce92b129b95" width="1200" />
</div>

  <div align="center">
  <img src="https://github.com/user-attachments/assets/aca41c5c-88c1-4fae-a55e-a93f73cc701f" width="1200" />
</div>

  
  

### Soal 4

> Dosen meminta Budiman membuat sistem operasi ini memilki **superuser** layaknya sistem operasi pada umumnya. User root yang sudah kamu buat sebelumnya akan digunakan sebagai superuser dalam sistem operasi milik Budiman. Superuser yang dimaksud adalah user dengan otoritas penuh yang dapat mengakses seluruhnya. Akan tetapi user lain tidak boleh memiliki otoritas yang sama. Dengan begitu user-user selain root tidak boleh mengakses `./root`. Buatlah sehingga tiap user selain superuser tidak dapat mengakses `./root`!

> _The lecturer requests that the OS must have a **superuser** just like other operating systems. The root user created earlier will serve as the superuser in Budiman's OS. The superuser should have full authority to access everything. However, other users should not have the same authority. Therefore, users other than root should not be able to access `./root`. Implement this so that non-superuser accounts cannot access `./root`!_

**Answer:**

- **Code:**
  
  Code 1
  ```
  ls -ld ~/osboot/BudiBebanKelompok/home/root
  ```
  Code 2
  ```
  chown username:username /path/to/home/username
  chmod 700 /path/to/home/username
  ```

- **Explanation:**

  Pada tahap ini, saya tidak perlu lagi membuat user root menjadi superuser, karena secara default user root memang sudah merupakan superuser dengan UID 0. Hal ini juga telah diperkuat sebelumnya di soal nomor 3, di mana user root dimasukkan ke dalam grup dengan GID 0 pada file etc/group. Jadi, root sudah memiliki hak akses penuh ke seluruh sistem.
  
  Untuk memastikan bahwa hanya user root yang memiliki akses terhadap direktori /root, saya pertama-tama memverifikasi permission-nya menggunakan perintah pada code 1, guna melihat apakah direktori tersebut memang sudah dibatasi aksesnya. Hasil dari perintah tersebut menunjukkan bahwa direktori belum sepenuhnya eksklusif untuk root.

  Oleh karena itu, pada code 2, saya melakukan dua langkah: mengatur kepemilikan direktori home/root agar dimiliki oleh user dan grup root, dan kemudian mengatur permission-nya agar hanya root yang dapat mengaksesnya. Dengan begitu, seluruh user selain root, termasuk Budiman, guest, praktikan1, dan praktikan2, tidak akan bisa mengakses direktori ./root.
Langkah ini saya lakukan untuk memastikan bahwa dalam sistem operasi ini, hanya superuser yang memiliki akses penuh, dan user lain tidak memiliki otoritas yang sama sebagaimana diminta dalam soal.
  

- **Screenshot:**

  <div align="center">
  <img src="https://github.com/user-attachments/assets/98e399bc-8788-4e76-b75f-a3a926a6447b" width="1200" />
</div>
  <div align="center">
  <img src="https://github.com/user-attachments/assets/534b0719-607f-4514-b5a9-9c9b9320acff" width="1200" />
</div>

### Soal 5

> Setiap user rencananya akan digunakan oleh satu orang tertentu. **Privasi dan otoritas tiap user** merupakan hal penting. Oleh karena itu, Budiman ingin membuat setiap user hanya bisa mengakses dirinya sendiri dan tidak bisa mengakses user lain. Buatlah sehingga sistem operasi Budiman dalam melakukan hal tersebut!

> _Each user is intended for an individual. **Privacy and authority** for each user are important. Therefore, Budiman wants to ensure that each user can only access their own files and not those of others. Implement this in Budiman's OS!_

**Answer:**

- **Code:**

  ```
  chown 1000:100 ~/osboot/BudiBebanKelompok/home/Budiman
  chmod 700 ~/osboot/BudiBebanKelompok/home/Budiman

  chown 1001:100 ~/osboot/BudiBebanKelompok/home/guest
  chmod 700 ~/osboot/BudiBebanKelompok/home/guest

  chown 1002:100 ~/osboot/BudiBebanKelompok/home/praktikan1
  chmod 700 ~/osboot/BudiBebanKelompok/home/praktikan1

  chown 1003:100 ~/osboot/BudiBebanKelompok/home/praktikan2
  chmod 700 ~/osboot/BudiBebanKelompok/home/praktikan2
  ```

- **Explanation:**

  Pada tahap ini, saya mengatur agar setiap user hanya dapat mengakses direktori miliknya sendiri. Langkah ini dilakukan dengan menetapkan kepemilikan direktori home masing-masing user berdasarkan UID dan GID yang telah didefinisikan sebelumnya. Setelah itu, saya memberikan permission 700 pada direktori tersebut.

  Permission 700 berarti hanya pemilik direktori yang dapat membaca, menulis, dan masuk ke dalam direktori tersebut. Dengan demikian, user lain tidak akan bisa mengakses isi direktori user lain, meskipun mereka berada dalam grup yang sama.

  Pengaturan ini memastikan privasi dan pembatasan akses antar pengguna sesuai dengan prinsip sistem multiuser. Setiap user hanya dapat mengakses file dan direktori miliknya sendiri, sementara user lain tidak memiliki izin untuk mengakses direktori tersebut. Hal ini sesuai dengan permintaan soal untuk menjamin privasi dan otoritas masing-masing user.

- **Screenshot:**


  <div align="center">
  <img src="https://github.com/user-attachments/assets/2bb21699-b4d7-49e3-9259-7bd4de237412" width="1200" />
</div>

### Soal 6

> Dosen Budiman menginginkan sistem operasi yang **stylish**. Budiman memiliki ide untuk membuat sistem operasinya menjadi stylish. Ia meminta kamu untuk menambahkan tampilan sebuah banner yang ditampilkan setelah suatu user login ke dalam sistem operasi Budiman. Banner yang diinginkan Budiman adalah tulisan `"Welcome to OS'25"` dalam bentuk **ASCII Art**. Buatkanlah banner tersebut supaya Budiman senang! (Hint: gunakan text to ASCII Art Generator)

> _Budiman wants a **stylish** operating system. Budiman has an idea to make his OS stylish. He asks you to add a banner that appears after a user logs in. The banner should say `"Welcome to OS'25"` in **ASCII Art**. Use a text to ASCII Art generator to make Budiman happy!_ (Hint: use a text to ASCII Art generator)

**Answer:**

- **Code:**
  
  Code 1
  ```
  cat > ~/osboot/BudiBebanKelompok/etc/banner.txt <<'EOF'
   _    _      _                            _          _____ _____ _ _____  _____ 
  | |  | |    | |                          | |        |  _  /  ___( ) __  \|  ___|
  | |  | | ___| | ___ ___  _ __ ___   ___  | |_ ___   | | | \ `--.|/`' / /'|___ \ 
  | |/\| |/ _ \ |/ __/ _ \| '_ ` _ \ / _ \ | __/ _ \  | | | |`--. \   / /      \ \
  \  /\  /  __/ | (_| (_) | | | | | |  __/ | || (_) | \ \_/ /\__/ / ./ /___/\__/ /
   \/  \/ \___|_|\___\___/|_| |_| |_|\___|  \__\___/   \___/\____/  \_____/\____/ 
  EOF
  ```
  Code 2
  ```
  chmod 644 ~/osboot/BudiBebanKelompok/etc/banner.txt
  ```
  Code 3
  ```
  cat > ~/osboot/BudiBebanKelompok/init <<'EOF'
  #!/bin/sh

  mount -t proc none /proc
  mount -t sysfs none /sys
  mount -t devtmpfs none /dev

  hostname BudiBebanOS

  clear
  cat /etc/banner.txt
  echo -e "\nBooting BudiBebanOS...\n"

  for tty in tty1 tty2 tty3 tty4 tty5
  do
      while true; do
          /bin/getty -L $tty 115200 vt100
          sleep 1
      done &
  done

  wait
  EOF
  ```
  Code 4
  ```
  chmod +x ~/osboot/BudiBebanKelompok/init
  ```
  Code 5
  ```
  cd ~/osboot/BudiBebanKelompok
  find . | cpio -oHnewc | gzip > ../BudiBebanKelompok.gz
  ```

- **Explanation:**

  Untuk menampilkan banner bertuliskan "Welcome to OS'25" dalam bentuk ASCII Art saat proses booting, dilakukan lima tahapan berikut:
  Pada code 1, dibuat file banner.txt yang berisi teks ASCII Art sesuai permintaan. File ini disimpan di dalam direktori /etc agar dapat diakses oleh proses boot.
  Selanjutnya, pada code 2, file banner.txt diberikan hak akses baca (read) agar dapat dibaca oleh skrip init saat sistem menyala.
  Pada code 3, skrip init dimodifikasi dengan menambahkan satu baris perintah cat /etc/banner.txt setelah proses mounting. Tujuannya adalah agar banner tampil pada layar sebelum proses booting dilanjutkan. Hanya satu baris ini yang ditambahkan untuk menjaga skrip tetap sederhana dan sesuai fungsi.
  Kemudian, pada code 4, file init diberikan hak akses eksekusi agar dapat dijalankan oleh sistem saat proses inisialisasi.
  Terakhir, pada code 5, seluruh isi direktori sistem dikompresi ulang menggunakan cpio dan gzip untuk menghasilkan berkas initramfs yang akan digunakan saat proses boot berlangsung.
  Dengan tahapan-tahapan ini, banner ASCII Art ditampilkan secara otomatis setiap kali sistem melakukan booting, sesuai dengan permintaan soal.

- **Screenshot:**

  <div align="center">
  <img src="https://github.com/user-attachments/assets/eb81632a-b36a-4005-a9e1-bcd1da4739c0" width="1200" />
</div>

### Soal 7

> Melihat perkembangan sistem operasi milik Budiman, Dosen kagum dengan adanya banner yang telah kamu buat sebelumnya. Kemudian Dosen juga menginginkan sistem operasi Budiman untuk dapat menampilkan **kata sambutan** dengan menyebut nama user yang login. Sambutan yang dimaksud berupa kalimat `"Helloo %USER"` dengan `%USER` merupakan nama user yang sedang menggunakan sistem operasi. Kalimat sambutan ini ditampilkan setelah user login dan setelah banner. Budiman kembali lagi meminta bantuanmu dalam menambahkan fitur ini.

> _Seeing the progress of Budiman's OS, the lecturer is impressed with the banner you created. The lecturer also wants the OS to display a **greeting message** that includes the name of the user who logs in. The greeting should say `"Helloo %USER"` where `%USER` is the name of the user currently using the OS. This greeting should be displayed after user login and after the banner. Budiman asks for your help again to add this feature._

**Answer:**

- **Code:**

Code 1
  ```
  cat > ~/osboot/BudiBebanKelompok/etc/profile <<'EOF'
  #!/bin/sh
  echo ""
  echo "Helloo $(whoami)!"
  echo "Waktu: $(date)"
  echo ""
  EOF
  ```
  Code 2
  ```
  chmod 755 ~/osboot/BudiBebanKelompok/etc/profile
  ```
  Code 3
  ```
  cd ~/osboot/BudiBebanKelompok
  find . | cpio -oHnewc | gzip > ../BudiBebanKelompok.gz
  ```

- **Explanation:**

  Untuk menambahkan fitur kata sambutan setelah login, dilakukan beberapa tahapan konfigurasi sebagai berikut:

  Pada code 1, dibuat file profile di dalam direktori /etc. File ini berfungsi sebagai skrip yang akan dijalankan secara otomatis setiap kali seorang pengguna berhasil login melalui terminal. Di dalam skrip  tersebut ditambahkan perintah echo "Helloo $(whoami)!" untuk menyapa pengguna berdasarkan nama login yang aktif, serta menampilkan waktu login melalui perintah date.
  Selanjutnya, pada code 2, file profile diberikan hak akses eksekusi agar dapat dijalankan oleh shell login secara otomatis. Hak akses ini penting agar sistem dapat mengeksekusi skrip tersebut tanpa kendala.
  Terakhir, pada code 3, seluruh isi direktori sistem dikompresi ulang menjadi berkas initramfs menggunakan perintah cpio dan gzip. Hal ini diperlukan agar perubahan yang dilakukan terhadap sistem dapat diterapkan saat proses booting berikutnya.
  Dengan langkah-langkah ini, sistem akan menampilkan kalimat sambutan yang bersifat personal setiap kali pengguna login, sebagaimana diminta dalam soal.

- **Screenshot:**

  <div align="center">
  <img src="https://github.com/user-attachments/assets/fe17f2ae-225d-4bee-8b51-4857f7ae28ee" width="1200" />
</div>


### Soal 8

> Dosen Budiman sudah tua sekali, sehingga beliau memiliki kesulitan untuk melihat tampilan terminal default. Budiman menginisiatif untuk membuat tampilan sistem operasi menjadi seperti terminal milikmu. Modifikasilah sistem operasi Budiman menjadi menggunakan tampilan terminal kalian.

> _Budiman's lecturer is quite old and has difficulty seeing the default terminal display. Budiman takes the initiative to make the OS look like your terminal. Modify Budiman's OS to use your terminal appearance!_

**Answer:**

- **Code:**

Code 1
  ```
  echo $TERM
  ```
  Code 2
  ```
  cat > ~/osboot/BudiBebanKelompok/init <<'EOF'
  #!/bin/sh

  mount -t proc none /proc
  mount -t sysfs none /sys
  mount -t devtmpfs none /dev

  hostname BudiBebanOS

  export TERM=xterm-256color
  export COLORTERM=truecolor

  clear
  cat /etc/banner.txt
  echo -e "\nSystem initializing...\n"

  for tty in tty1 tty2 tty3 tty4 tty5
  do
      while true; do
          /bin/getty -L $tty 115200 xterm-256color
          sleep 1
      done &
  done
  
  wait
  EOF
  ```
  Code 3
  ```
  cat > ~/osboot/BudiBebanKelompok/etc/profile <<'EOF'
  #!/bin/sh
  
  export TERM=xterm-256color
  export PS1='\[\e[1;32m\]\u@\h\[\e[0m\]:\[\e[1;34m\]\w\[\e[0m\]\$ '
  alias ls='ls --color=auto'
  
  echo -e "\e[1;36m"
  cat /etc/banner.txt
  echo -e "\e[0m"

  echo -e "\e[1;33mHelloo \e[1;35m$(whoami)\e[0m!"
  echo -e "Waktu: \e[1;32m$(date)\e[0m"
  echo ""
  EOF
  ```
Code 4
  ```
  mkdir -p ~/osboot/BudiBebanKelompok/etc/skel
  cat > ~/osboot/BudiBebanKelompok/etc/skel/.bashrc <<'EOF'

  if [ "$TERM" = "xterm-256color" ]; then
      alias ls='ls --color=auto'
      alias grep='grep --color=auto'
      alias fgrep='fgrep --color=auto'
      alias egrep='egrep --color=auto'
  fi
  EOF
  ```
  Code 5
  ```
  cd ~/osboot/BudiBebanKelompok
  find . | cpio -oHnewc | gzip > ../BudiBebanKelompok.gz
  ```

- **Explanation:**

  Untuk meningkatkan keterbacaan tampilan terminal bagi Dosen Budiman, dilakukan serangkaian konfigurasi agar sistem operasi menggunakan profil terminal xterm-256color dengan dukungan warna penuh. Penyesuaian ini dilakukan melalui lima tahap berikut:

  Pada code 1, dilakukan identifikasi jenis terminal yang digunakan oleh sistem pengembang menggunakan perintah echo $TERM. 

  Selanjutnya, pada code 2, dilakukan modifikasi terhadap file init dengan menambahkan dua baris ekspor variabel lingkungan: TERM=xterm-256color dan COLORTERM=truecolor. Nilai-nilai ini menetapkan standar tampilan terminal selama proses boot berlangsung. Selain itu, program getty juga dijalankan dengan argumen terminal yang sesuai agar seluruh terminal virtual menggunakan konfigurasi warna tersebut.

  Pada code 3, file profile diperbarui untuk memberikan tampilan berwarna setelah login. Variabel PS1 diatur ulang agar prompt pengguna tampil dengan kombinasi warna yang kontras. Selain itu, alias ls ditambahkan agar hasil daftar direktori berwarna. Banner ditampilkan dengan warna cyan, dan sambutan login dicetak dengan warna yang berbeda-beda untuk meningkatkan visibilitas.

  Kemudian, pada code 4, dibuat file .bashrc di dalam direktori etc/skel. File ini akan disalin secara otomatis ke direktori home pengguna baru. Di dalamnya terdapat konfigurasi alias dan dukungan warna tambahan untuk perintah seperti grep, fgrep, dan egrep, yang akan aktif jika terminal mendukung warna.

  Terakhir, pada code 5, disalin beberapa file pendukung dari sistem host ke dalam sistem operasi Budiman, yakni berkas biner ls dan file terminfo untuk xterm-256color. Hal ini penting agar perintah ls berfungsi dengan benar dan terminal dapat mengenali tipe warna yang digunakan.

  Dengan seluruh tahapan tersebut, sistem operasi Budiman kini mendukung tampilan terminal berwarna yang lebih jelas dan mudah dibaca, sehingga lebih ramah untuk pengguna lansia seperti Dosen Budiman.

- **Screenshot:**

  <div align="center">
  <img src="https://github.com/user-attachments/assets/8bb15a29-ce44-4a13-97b0-7a5f75a6eb80" width="1200" />
</div>
  <div align="center">
  <img src="https://github.com/user-attachments/assets/11d1253a-0a55-45fe-93e7-053c25a2b7c8" width="1200" />
</div>

### Soal 9

> Ketika mencoba sistem operasi buatanmu, Budiman tidak bisa mengubah text file menggunakan text editor. Budiman pun menyadari bahwa dalam sistem operasi yang kamu buat tidak memiliki text editor. Budimanpun menyuruhmu untuk menambahkan **binary** yang telah disiapkan sebelumnya ke dalam sistem operasinya. Buatlah sehingga sistem operasi Budiman memiliki **binary text editor** yang telah disiapkan!

> _When trying your OS, Budiman cannot edit text files using a text editor. He realizes that the OS you created does not have a text editor. Budiman asks you to add the prepared **binary** into his OS. Make sure Budiman's OS has the prepared **text editor binary**!_

**Answer:**

- **Code:**
  Code 1
  ```
  cp /usr/bin/nano /home/rei/osboot/BudiBebanKelompok/bin/
  ```
  Code 2
  ```
  chmod +x /home/rei/osboot/BudiBebanKelompok/bin/texteditor
  ```
  Code 3
  ```
  nano /home/rei/osboot/BudiBebanKelompok/etc/profile
  #!/bin/sh
  
  export TERM=xterm-256color
  export PS1='\[\e[1;32m\]\u@\h\[\e[0m\]:\[\e[1;34m\]\w\[\e[0m\]\$ '
  alias ls='ls --color=auto'
  
  echo -e "\e[1;36m"
  cat /etc/banner.txt
  echo -e "\e[0m"

  echo -e "\e[1;33mHelloo \e[1;35m$(whoami)\e[0m!"
  echo -e "Waktu: \e[1;32m$(date)\e[0m"
  echo ""

  alias edit='nano'

  ```

- **Explanation:**

  Dalam proses pengembangan sistem operasi minimal berbasis Linux, salah satu permasalahan yang ditemukan adalah tidak tersedianya fasilitas untuk melakukan penyuntingan berkas teks secara langsung di dalam sistem. Hal ini menjadi kendala ketika pengguna, dalam hal ini Budiman, ingin mengubah isi berkas teks namun tidak dapat melakukannya karena sistem belum dilengkapi dengan text editor bawaan. Untuk itu, diperlukan langkah tambahan guna mengintegrasikan text editor ke dalam lingkungan sistem operasi tersebut.

  Langkah pertama yang dilakukan adalah menyalin berkas binary dari aplikasi nano, yang merupakan salah satu text editor terminal ringan dan umum digunakan, ke dalam direktori root filesystem dari sistem operasi Budiman. Proses ini tercantum pada Code 1 dan bertujuan agar binary nano tersedia dan dapat dieksekusi langsung di dalam sistem operasi saat dijalankan.

  Selanjutnya, untuk memastikan binary yang telah ditambahkan tersebut memiliki hak akses eksekusi yang sesuai, dilakukan penyesuaian permission sebagaimana dijelaskan dalam Code 2. Hal ini penting agar pengguna sistem dapat menjalankan program nano tanpa kendala hak akses.

  Agar penggunaan text editor ini lebih intuitif bagi pengguna, kemudian dilakukan modifikasi terhadap berkas konfigurasi shell login yang berada di dalam root filesystem sistem operasi, yaitu berkas profile. Dalam modifikasi ini (Code 3), ditambahkan alias edit yang merujuk langsung pada program nano, sehingga pengguna cukup mengetikkan perintah edit untuk memanggil text editor. Selain itu, pada berkas konfigurasi tersebut juga dilakukan penyesuaian terhadap tampilan shell seperti warna teks dan penambahan sambutan saat login guna meningkatkan pengalaman pengguna.

  Dengan dilaksanakannya ketiga tahapan tersebut, maka sistem operasi Budiman kini telah dilengkapi dengan text editor fungsional yang dapat digunakan untuk menyunting berkas teks secara langsung di lingkungan terminal, sesuai dengan kebutuhan pengguna dan permintaan soal.

- **Screenshot:**

  <div align="center">
  <img src="https://github.com/user-attachments/assets/7b4e1a7c-f57a-4be6-bd4c-a4a5d7328218" width="1200" />
</div>

### Soal 10

> Setelah seluruh fitur yang diminta Dosen dipenuhi dalam sistem operasi Budiman, sudah waktunya Budiman mengumpulkan tugasnya ini ke Dosen. Akan tetapi, Dosen Budiman tidak mau menerima pengumpulan selain dalam bentuk **.iso**. Untuk terakhir kalinya, Budiman meminta tolong kepadamu untuk mengubah seluruh konfigurasi sistem operasi yang telah kamu buat menjadi sebuah **file .iso**.

> After all the features requested by the lecturer have been implemented in Budiman's OS, it's time for Budiman to submit his assignment. However, Budiman's lecturer only accepts submissions in the form of **.iso** files. For the last time, Budiman asks for your help to convert the entire configuration of the OS you created into a **.iso file**.

**Answer:**

- **Code:**
  Code 1
  ```
  cd /home/rei/osboot
  ```
   Code 2
  ```
   mkdir -p mylinuxiso/boot/grub
  ```
  Code 3
  ```
  cp bzImage mylinuxiso/boot/
  cp BudiBebanKelompok.gz mylinuxiso/boot/
  ```
  Code 4
  ```
  nano mylinuxiso/boot/grub/grub.cfg
  set timeout=5
  set default=0
  
  menuentry "Budiman OS" {
    linux /boot/bzImage
    initrd /boot/BudiBebanKelompok.gz
  }
  ```
  Code 5
  ```
  grub-mkrescue -o BudimanOS.iso mylinuxiso
  ```
  
- **Explanation:**

  Setelah seluruh fungsionalitas utama dari sistem operasi Budiman berhasil diimplementasikan, tahapan akhir dari pengembangan ini adalah melakukan proses packaging sistem ke dalam bentuk berkas image .iso. Hal ini dilakukan untuk memenuhi ketentuan pengumpulan tugas dari pihak dosen yang mensyaratkan bentuk akhir berupa berkas image bootable, yang dapat dijalankan melalui emulator ataupun ditulis ke media fisik seperti USB dan CD/DVD.

  Proses konversi konfigurasi sistem operasi menjadi file .iso diawali dengan memastikan bahwa seluruh kegiatan dilakukan pada direktori kerja utama, sebagaimana ditunjukkan pada Code 1. Selanjutnya, dibentuklah struktur direktori yang menyerupai sistem bootable standar, yakni direktori boot dan subdirektori grub sebagai lokasi penyimpanan bootloader GRUB (Code 2). Struktur ini sangat penting karena GRUB memerlukan format direktori tertentu agar dapat dikenali saat proses booting.

  Langkah berikutnya adalah menyalin file kernel (bzImage) dan root filesystem dalam bentuk compressed cpio archive (BudiBebanKelompok.gz) ke dalam direktori boot. Proses ini dijelaskan dalam Code 3, di mana kedua file tersebut merupakan komponen utama yang akan dijalankan oleh bootloader.

  Agar sistem dapat diboot secara otomatis dan terkonfigurasi dengan benar, dibuatlah berkas konfigurasi grub.cfg sebagaimana dijelaskan pada Code 4. Berkas ini berfungsi untuk memberikan instruksi kepada GRUB mengenai kernel yang akan dijalankan dan initrd yang harus dimuat selama proses boot. Selain itu, konfigurasi ini juga menentukan tampilan menu awal serta pilihan default yang akan dijalankan secara otomatis.

  Tahapan terakhir adalah melakukan pembuatan berkas .iso menggunakan tool grub-mkrescue sebagaimana tertera pada Code 5. Perintah ini akan menggabungkan seluruh struktur direktori, kernel, initrd, serta konfigurasi GRUB ke dalam satu berkas .iso yang dapat langsung digunakan sebagai media bootable.

- **Screenshot:**

  <div align="center">
  <img src="https://github.com/user-attachments/assets/10455f48-67cc-44b8-8527-037b9243e22e" width="1200" />
</div>

---

> End product
    <div align="center">
  <img src="https://github.com/user-attachments/assets/74cc614e-2975-4e88-886c-549b9944e40f" width="1200" />
</div>


Pada akhirnya sistem operasi Budiman yang telah kamu buat dengan susah payah dikumpulkan ke Dosen mengatasnamakan Budiman. Kamu tidak diberikan credit apapun. Budiman pun tidak memberikan kata terimakasih kepadamu. Kamupun kecewa tetapi setidaknya kamu telah belajar untuk menjadi pembuat sistem operasi sederhana yang andal. Selamat!

_At last, the OS you painstakingly created was submitted to the lecturer under Budiman's name. You received no credit. Budiman didn't even thank you. You feel disappointed, but at least you've learned to become a reliable creator of simple operating systems. Congratulations!_
