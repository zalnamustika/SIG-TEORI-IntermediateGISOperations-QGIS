# QGIS-TEORI-Intermediate-GIS-Operations
 Sistem Informasi Geografi Teori
 
 ## Project 1 -- Performing Table Joins 
1. Temukan tl_2019_06_tract.zipfile di Peramban QGIS dan kembangkan. Pilih tl_2019_06_tract.shpfile dan seret ke kanvas.

2. Dialog Select Transformation akan meminta untuk mengkonversi dari EPSG:4269 ke EPSG:4326 . Dialog ini menyajikan beberapa transformasi untuk mengkonversi antara koordinat antara proyeksi tersebut. Tinggalkan pilihan ke pilihan default dan klik OK .

3. Anda akan melihat layer tl_2019_06_tractdimuat di panel Layers . Lapisan ini berisi batas-batas bidang sensus di California. Klik kanan pada tl_2019_06_tractlayer dan pilih Open Attribute Table .

4. Periksa atribut layer. Untuk menggabungkan tabel dengan lapisan ini, kita memerlukan atribut unik dan umum dari setiap fitur. Dalam hal ini, ada 8057 catatan traktat individu dengan GEOIDbidang tersebut. Kolom ini dapat menautkan lapisan ini dengan lapisan atau tabel lain yang berisi ID yang sama.

5. Untuk memuat data tabular, klik Open Data Source Manager .

6. Dalam dialog Pengelola Sumber Data , pilih Teks yang Dibatasi . Kemudian di sebelah kanan, klik di ...sebelah Nama file dan ramban ke folder yang tidak di-zip dengan CSV populasi California.

7. Sekarang di bawah Sample Data , kita dapat memeriksa data bahkan sebelum memuatnya sebagai layer. Representasi menunjukkan bahwa tabel data berisi 2 baris header.

8. Untuk menghilangkan baris header tambahan, di bawah Opsi Rekam dan Bidang atur Jumlah baris header yang akan dibuang ke 1. Sekarang tabel akan berisi tajuk kolom yang tepat. Karena layer ini hanya berisi data tabular, pilih di bawah Geometry Definition . Klik Add untuk menambahkannya sebagai layer dan kemudian klik Close untuk menutup kotak dialog ini.No geometry (attribute only table)

9. CSV sekarang akan diimpor sebagai tabel ke QGIS dan muncul seperti ACST5Y2019.S0101pada panel Lapisan . Sekarang klik kanan pada layer dan pilih Open Attribute Table .

10. Kolom IDtersebut berisi id unik untuk setiap record, yang dapat digunakan untuk menggabungkan tabel ini dengan tl_2019_06_tractlayer. Jika Anda membandingkan nilai IDdengan GEOIDkolom dari tl_2019_06_tract. Anda akan melihat bahwa itu diawali dengan 1400000US . Untuk menggabungkan kedua tabel ini dengan sukses, nilainya harus sama persis. Mari kita hapus awalan ini dan tambahkan kolom baru dengan 11 karakter terakhir yang berisi nilai yang sama persis.

11. Untuk membuat kolom baru dengan 11 digit terakhir, buka Processing Toolbox dengan masuk ke Processing ‣ Toolbox , dan cari dan temukan tabel Vector ‣ Algoritma kalkulator lapangan.

12. Dalam dialog Kalkulator bidangACST5Y2019.S0101 , pilih sebagai Input layer , masukkan Nama bidanggeoid , dan pilih Jenis Bidang Hasil . Sekarang cari dalam ekspresi. Kita dapat menggunakan fungsi ini untuk mengekstrak bagian yang diperlukan dari bidang id.stringsubstr.

13. Masukkan ekspresi di bawah ini. Kami menggunakan fungsi substr dan mengekstrak nilai dari posisi -11 (nilai negatif dihitung dari akhir). Hasil akhir dapat dilihat di bagian Pratinjau . Klik Jalankan .
------------------

substr("id", -11)

------------------
 
 14. Sekarang layer baru Calculatedakan dimuat di kanvas, mari kita periksa tabel atribut. Kolom baru geoiddengan nilai yang dapat dicocokkan dengan risalah pencacahan akan hadir.

15. Untuk membuat gabungan tabel, buka Processing Toolbox dengan masuk ke Processing ‣ Toolbox , dan cari dan temukan atribut Vector general ‣ Join dengan algoritma nilai bidang.

16. Dalam dialog Gabungkan atribut menurut nilai bidangtl_2019_06_tract , pilih sebagai lapisan Input dan GEOIDsebagai bidang Tabel . Pilih Calculatedsebagai Input layer 2 dan geoidsebagai Table field 2 . Di bawah bidang Layer2 untuk menyalin , klik pada ....

17. Periksa , dan . Klik Oke .Geographic Area NameEstimate!!Total!!Total populationgeoid

18. Centang catatan Buang yang tidak dapat digabungkan . Ini akan menghilangkan catatan tambahan apa pun di tabel populasi. Klik tombol … di bawah layer gabungan untuk memilih lokasi file keluaran dan pilih .Save to File...

19. Beri nama geopackage keluaran sebagai california_total_population.gpkg. Klik Jalankan .

20. Setelah pemrosesan selesai, verifikasi bahwa algoritme berhasil jika semua fitur 8057 digabungkan. Klik Tutup .

21. Anda akan melihat layer baru california_total_populationdimuat di panel Layers . Pada titik ini, bidang dari file CSV digabungkan dengan lapisan saluran sensus. Sekarang setelah kita memiliki data kependudukan di lapisan sensus, kita dapat menatanya untuk membuat visualisasi distribusi kepadatan penduduk. Klik tombol Buka Panel Penataan Lapisan .

22. Di panel Layer Styling , pilih Graduateddari menu drop-down. Saat kami ingin membuat peta kepadatan populasi, kami ingin menetapkan warna berbeda untuk setiap fitur saluran sensus berdasarkan kepadatan populasi. Kami memiliki populasi di kolom Estimasi!!Total!!Total populasi , dan area area di ALAND . Klik tombol Ekspresi , untuk menghitung persentase jumlah penduduk pada setiap saluran pencacahan.
----------------------------

Catatan:
Saat membuat peta tematik (choropleth) seperti ini, penting untuk menormalkan nilai yang Anda petakan. Memetakan jumlah total per poligon tidak benar. Penting untuk menormalkan nilai-nilai yang dibagi dengan luas. Jika Anda menampilkan total seperti kejahatan, Anda dapat menormalkannya dengan membaginya dengan total populasi, sehingga memetakan tingkat kejahatan dan bukan kejahatan.

----------------------------

23. Masukkan ekspresi berikut untuk menghitung kepadatan populasi. Area fitur diberikan dalam kilometer persegi. Kami kemudian mengubahnya menjadi meter persegi dengan mengalikan dengan 1000000dan menghitung kepadatan penduduk dengan rumus Penduduk/Luas . Pratinjau hasilnya dan klik OK .
-------------------------

1000000 * ("Estimate!!Total!!Total population"/"ALAND")

-------------------------

24. Di Panel Penataan Lapisan , klik klasifikasikan dan masukkan kelas sebagai 10.

25. Klik pada jalur warna untuk memilih jalur warna RdYlGn.

26. Kerapatan yang lebih tinggi lebih penting, mari kita tetapkan warna hijau untuk kerapatan rendah dan merah untuk area dengan kerapatan tinggi. Klik pada jalur warna dan pilih Balikkan Jalur Warna .

27. Sekarang kami memiliki visualisasi informasi kepadatan populasi yang sangat baik di California. Untuk membuatnya lebih baik, mari kita buat batas setiap saluran sensus transparan. Klik pada tab Simbol.

28. Klik pada warna Stroke dan klik .Transparent stroke

29. Tempat sampah dapat disesuaikan, klik pada Nilai ini akan memunculkan dialog untuk memasukkan nilai batas atas dan bawah.

30. Setelah Anda puas menutup panel styling Layer. Kami sekarang memiliki visualisasi informasi kepadatan populasi yang terlihat bagus di California.
 
============================================================================================
 
## Project 2 -- Performing Spatial Joins

1. Temukan nybb_19a.zipfile di Peramban QGIS dan kembangkan. Pilih nybb_19a/nybb.shplayer dan seret ke kanvas. Ini adalah lapisan poligon yang mewakili batas wilayah di kota New York.

2. Selanjutnya, cari V_SSS_SEGMENTRATING_1.zipfile dan perluas. Pilih dot_V_SSS_SEGMENTRATING_1_20190129.shplayer dan tambahkan ke kanvas. Ini adalah lapisan garis dari semua jalan di kota.

3. Mari kita periksa atribut yang tersedia untuk setiap fitur dot_V_SSS_SEGMENTRATING_1_20190129layer. Klik kanan dan pilih Open Attribute Table .

4. Anda akan melihat atribut yang dipanggil Rating_Byang memiliki nilai dalam rentang 0-10 yang mewakili peringkat segmen jalan. Atribut RatingWordmemiliki peringkat deskriptif. Kita dapat menggunakan Rating_Bbidang tersebut untuk menghitung peringkat rata-rata.

5. Anda mungkin telah memperhatikan bahwa beberapa fitur memiliki peringkat NR. Ini adalah segmen yang tidak diberi peringkat. Memasukkan mereka ke dalam analisis kami tidak akan benar. Sebelum kita melakukan penggabungan spasial, mari siapkan Filter untuk mengecualikan rekaman ini. Klik kanan dot_V_SSS_SEGMENTRATING_1_20190129layer dan pilih Filter .

6. Di Pembuat Kueri , ketikkan ekspresi berikut untuk memilih semua rekaman yang tidak diberi peringkat NR. Anda juga dapat membuat ekspresi secara interaktif dengan mengklik Field , Operator dan memilih Value yang sesuai . Klik Oke .

------------------------

"RatingWord" != 'NR'

------------------------

7. Anda akan melihat dot_V_SSS_SEGMENTRATING_1_20190129layer sekarang memiliki ikon filter yang menunjukkan bahwa ada filter aktif yang diterapkan ke layer ini. Sekarang kita bisa melakukan penggabungan spasial menggunakan layer ini. Pergi ke Memproses ‣ Kotak Alat .

8. Cari dan temukan Vector general ‣ Gabungkan atribut berdasarkan algoritma lokasi (ringkasan). Klik dua kali untuk meluncurkannya.

9. Dalam dialog Gabungkan atribut berdasarkan lokasi (ringkasan)nybb , pilih sebagai Input layer . Lapisan jalan dot_V_SSS_SEGMENTRATING_1_20190129akan menjadi lapisan Gabung . Anda dapat membiarkan predikat Geometri pada default Intersects. Klik tombol … di sebelah Fields to sumarize .

------------------------

Catatan :
Kiat untuk membantu Anda memilih masukan yang benar dan menggabungkan lapisan: Lapisan masukan adalah salah satu yang akan dimodifikasi dengan atribut baru dalam gabungan spasial. Karena kami ingin bidang peringkat rata-rata ditambahkan ke lapisan wilayah, itu akan menjadi lapisan masukan.

------------------------

10. Pilih Rating_Bdan klik OK .

11. Demikian pula, klik tombol … di sebelah Ringkasan untuk menghitung .

12. Pilih meansebagai operator ringkasan dan klik OK . Sekarang kita siap untuk memulai pemrosesan. Klik Jalankan .

13. Algoritme pemrosesan akan bekerja melalui fitur dan menerapkan gabungan spasial. Verifikasi bahwa pekerjaan pemrosesan berhasil dan klik Tutup .

14. Kembali ke jendela utama QGIS, Anda akan melihat layer baru ditambahkan ke kanvas. Buka tabel atribut untuk lapisan ini. Anda akan melihat kolom baru ditambahkan ke layer input borough dengan rating rata-rata semua jalan yang bersinggungan dengan fitur tersebut.Joined layerRating_B_mean

15. Sekarang kita dapat melakukan operasi terbalik. Terkadang analisis Anda memerlukan atribut dari lapisan lain berdasarkan hubungan spasial tetapi tidak menghitung ringkasan apa pun. Kita dapat menggunakan algoritma untuk analisis tersebut. Tugasnya adalah menambahkan nama borough ke setiap fitur di layer jalan berdasarkan poligon borough mana yang bersinggungan dengannya. Sebelum kita menjalankan algoritme ini, mari hapus filter dari layer. Klik ikon filter dan tekan Clear di Query Builder . Klik Oke .Join attribute by locationdot_V_SSS_SEGMENTRATING_1_20190129

16. Matikan di panel Lapisan . Temukan Vector general ‣ Gabung atribut berdasarkan algoritme lokasi di Processing Toolbox dan klik dua kali untuk meluncurkannya.Joined layer

17. Pilih dot_V_SSS_SEGMENTRATING_1_20190129sebagai layer Input dan nybbsebagai layer Join . Anda dapat membiarkan predikat Geometri pada default Intersects. Klik tombol … di sebelah Bidang untuk ditambahkan dan dipilih BoroName. Klik Oke .

18. Ruas garis dapat melintasi batas wilayah, jadi kami memilih tipe Gabungan sebagai . Klik Jalankan .Crate separate feature for each located feature (one-to-many)

19. Setelah pemrosesan selesai, buka tabel atribut file . Anda akan melihat bahwa ada atribut baru yang ditambahkan ke setiap fitur jalan.Joined layerBoroName

============================================================================================

## Project 3 -- Performing Spatial Queries

1. Temukan metro_stations_accessbility.zipfile di Peramban QGIS dan kembangkan. Pilih metro_stations_accessbility.shpfile dan seret ke kanvas. Lapisan baru metro_stations_accessbilityakan dimuat di panel Lapisan .

2. Lapisan data untuk bar dan pub dalam format CSV. Untuk memuatnya di QGIS, buka Layer ‣ Add Layer ‣ Add Delimited Text Layer… . ( Lihat Mengimpor File Spreadsheet atau CSV (QGIS3) untuk detail lebih lanjut tentang mengimpor file CSV)

3. Di Manajer Sumber Data | Dialog Teks Dibatasi , telusuri dan pilih file yang diunduh Bars_and_pubs__with_patron_capacity.csvsebagai Nama file . Bidang X dan kolom bidang Y harus dipilih secara otomatis ke dan masing- masing. Klik Tambahkan .x coordinatey coordinate

4. Anda akan melihat Bars_and_pubs__with_patron_capacitylayer baru ditambahkan ke panel Layers . Kedua layer input berada di Geograhpic Coordinate Reference System (CRS) . Untuk melakukan analisis spasial, disarankan untuk menggunakan Projected Coordinate Reference System (CRS). Jadi sekarang kita akan memproyeksikan ulang kedua layer ke CRS regional yang sesuai yang meminimalkan distorsi dan memungkinkan kita bekerja dalam satuan jarak seperti meter, bukan derajat. Pergi ke Memproses ‣ Kotak Alat .EPSG:4326 WGS84

5. Cari dan temukan Vector general ‣ Alat lapisan proyeksi ulang . Klik dua kali untuk meluncurkannya.

6. Pilih Bars_and_pubs__with_patron_capacitysebagai lapisan Input . Klik tombol Select CRS di sebelah Target CRS .

7. Saat memilih sistem koordinat yang diproyeksikan untuk analisis Anda, tempat pertama yang harus dicari adalah CRS regional untuk area yang diminati. Untuk Australia, Map Grid of Australia (MGA) 2020 adalah sistem grid berbasis UTM yang digunakan untuk pemetaan lokal dan regional. Melbourne termasuk dalam UTM Zone 55, jadi kita bisa memilih GDA 2020 / MGA zone 55 EPSG:7855` CRS.

--------------------------

Catatan

Jika Anda tidak yakin dengan CRS lokal untuk wilayah tempat Anda bekerja, memilih CRS untuk zona UTM berdasarkan datum WGS84 adalah pilihan yang aman. Anda dapat mengetahui nomor zona UTM wilayah Anda menggunakan UTM Grid Zones of the World .

--------------------------

8. Selanjutnya, klik tombol … di sebelah Diproyeksikan ulang dan pilih . Geopackage adalah data spasial format data terbuka yang direkomendasikan dan merupakan format pertukaran data default untuk QGIS3. Satu file GeoPackage dapat berisi beberapa layer vektor dan raster.Save to GeoPackage.gpkg

9. Beri nama geopackage sebagai spatialquerydan klik Save .

10. Saat diminta nama lapisan, masukkan bars_and_pubsdan klik OK . Klik Jalankan untuk memproyeksikan ulang lapisan.

11. Jendela akan beralih ke tab Log dan Anda akan melihat algoritme dijalankan dan membuat lapisan keluaran baru bars_and_pubs.

12. Sekarang kita akan memproyeksikan ulang metro_stations_accessbilitylayer. Beralih kembali ke tab Parameter di jendela Reproject layer . Pilih metro_stations_accessbilitysebagai lapisan Input . Pertahankan Target CRS yang sama . Selanjutnya, klik tombol … di sebelah Diproyeksikan ulang dan pilih . Pilih file keluaran yang sama (Ingat bahwa satu file geopackage dapat berisi banyak lapisan, jadi kami akan menyimpan lapisan baru ke file geopackage yang sama). Masukkan sebagai nama Layer . Klik Jalankan .Save to GeoPackagespatialquerymetro_stations

13. Kembali ke jendela utama QGIS, Anda akan melihat 2 layer baru dimuat di panel Layers : bars_and_pubsdan metro_stations. Anda dapat mematikan visibilitas untuk lapisan asli. Sekarang, kami siap untuk melakukan kueri spasial. Karena kami tertarik untuk memilih bar dan pub dalam jarak 500m dari stasiun metro, langkah pertama adalah membuat buffer di sekitar stasiun metro yang mewakili area pencarian kami. Cari dan temukan Vector geometry ‣ Buffer tool di Processing Toolbox dan klik dua kali untuk meluncurkannya.

14. Dalam dialog Buffermetro_stations , pilih sebagai Input layer . Tetapkan 500meter sebagai Jarak . Simpan hasilnya ke spatialquerygeopackage yang sama dan masukkan metro_stations_bufferssebagai Layer name . Klik Jalankan .

15. Anda akan melihat metro_stations_bufferslayer baru dimuat di panel Layers . Sekarang kita bisa mengetahui titik mana dari bars_and_pubslayer yang termasuk dalam poligon dari metro_stations_bufferslayer. Cari Vector selection ‣ Extract by Location tool dari Processing Toolbox dan klik dua kali untuk meluncurkannya.

--------------------------------------

Catatan

Ekstrak berdasarkan lokasi akan membuat layer baru dengan fitur yang cocok dari kueri spasial. Jika Anda hanya ingin memilih fitur, gunakan alat Pilih berdasarkan lokasi .

-------------------------------------

16. Dalam dialog Ekstrak berdasarkan lokasibars_and_pubs , pilih sebagai Ekstrak fitur dari . Periksa Intersectsebagai predikat geometri . Tetapkan metro_stations_bufferssebagai Dengan membandingkan fitur dari . Simpan hasilnya ke spatialquerygeopackage sebagai layer selected. Klik Jalankan .

17. Setelah pemrosesan selesai, Anda akan melihat selectedlayer ditambahkan ke panel Layers . Perhatikan bahwa lapisan ini hanya berisi titik-titik dari bars_and_pubsyang termasuk dalam poligon penyangga.

18. Analisis kami selesai. Anda mungkin memperhatikan bahwa poligon penyangga terlihat berbentuk oval. Ini karena Project CRS kita masih disetel ke EPSG:4326 WGS84 . Untuk memvisualisasikan hasil dengan lebih baik, Anda dapat pergi ke Proyek ‣ Properti ‣ CRS dan pilih mana yang kami gunakan untuk analisis. Setelah diatur ke CRS ini, buffer akan muncul dalam bentuk yang benar.GDA 2020 / MGA zone 55 EPSG:7855

============================================================================================

## Project 4 -- Nearest Neighbor Analysis

1. Temukan ne_10m_populated_places_simple.zipfile yang diunduh di panel Browser dan perluas. Seret ne_10m_populated_places_simple.shpfile ke kanvas.

2. Anda akan melihat layer baru ne_10m_populated_places_simpledimuat di panel Layers . Lapisan ini berisi titik-titik yang mewakili tempat berpenduduk. Sekarang kita akan memuat lapisan gempa bumi. Lapisan ini hadir sebagai file teks Tab Serepated Values ​​(TSV) . Untuk memuat file ini, klik tombol Open Data Source Manager di Data Source Toolbar . Anda juga dapat menggunakan pintasan keyboard.Ctrl + L

3. Di kotak dialog Pengelola Sumber Data , pilih Teks yang Dibatasi .

4. Klik tombol … di sebelah Nama file dan ramban ke earthquakes-2021-11-25_13-39-30_+0530.tsvfile yang diunduh. Bergantung pada sistem operasi, Anda mungkin tidak melihat file di direktori yang diunduh. Jika demikian, alihkan ke Semua file (*; .) dalam dialog Pilih File Teks yang Dibatasi untuk Dibuka . Setelah dibuka, pilih Pembatas khusus di bagian Format file , dan centang Tab. Di bagian Definisi geometri , pilih Koordinat titik . Secara default nilai bidang X dan bidang Y akan diisi secara otomatis dengan bidang yang sesuai di input. Dalam kasus kami, mereka adalah LongitudedanLatitude. Anda dapat meninggalkan CRS Geometri ke CRS default . Jika file Anda berisi koordinat dalam CRS yang berbeda, Anda dapat memilih CRS yang sesuai di sini. Klik Tambah diikuti oleh Tutup .EPSG:4326 - WGS 84

5. Perbesar dan jelajahi kedua set data. Setiap titik merah mewakili lokasi kejadian gempa bumi, dan setiap titik hijau mewakili lokasi tempat berpenduduk. Tujuan kita adalah menemukan titik terdekat dari lapisan tempat berpenduduk untuk setiap titik di lapisan gempa. Mari periksa tabel Atribut lapisan gempa bumi. Pilih layer dan klik ikon Open Attribute Table di Toolbar .

6. Ada 2586fitur, tetapi data berisi sedikit entri tanpa informasi lintang atau bujur. Kami harus menghapusnya sebelum melanjutkan lebih jauh. Tutup Tabel Atribut.

7. Pergi ke Pemrosesan ‣ Kotak Alat ‣ Geometri vektor ‣ Hapus alat geometri nol. Klik dua kali untuk membukanya.

8. Di kotak dialog Hapus Geometri Nullearthquakes-2021-11-25_13-39-30_+0530 , pilih sebagai lapisan Input dan centang kotak Juga hapus geometri kosong . Klik Jalankan . Setelah pemrosesan selesai, klik Tutup .

9. Layer baru akan ditambahkan ke panel Layers . Untuk analisis kita akan menggunakan layer ini sebagai pengganti layer aslinya. Hapus centang pada layer di panel Layers untuk menyembunyikannya. Pilih layer dan klik tombol Open Attribute Table dari Attributes Toolbar .Non null geometriesearthquakes-2021-11-25_13-39-30_+0530Non null geometries

10. Anda akan melihat jumlah fitur total yang lebih rendah karena semua baris dengan nilai lintang dan bujur kosong telah dihapus. Tutup tabel atribut.

11. Sekarang saatnya untuk melakukan analisis tetangga terdekat. Cari dan temukan Pemrosesan ‣ Toolbox ‣ Analisis vektor ‣ Jarak ke alat hub (baris ke hub) terdekat . Klik dua kali untuk meluncurkannya.

12. Di kotak dialog Distance to Nearest Hub (Line to Hub) , pilih sebagai layer Source points . Pilih sebagai layer Destination hubs . Pilih sebagai atribut nama lapisan Hub . Alat ini juga akan menghitung jarak garis lurus antara tempat berpenduduk dan gempa terdekat. Tetapkan sebagai unit Pengukuran . Klik di Jarak Hub dan klik Simpan ke File… untuk menyimpan file sebagai . Klik Jalankan . Setelah pemrosesan selesai, klik Tutup .Non null geometriesne_10m_populated_places_simplenameKilometers...earthquakes_with_nearest_city.gpkg

13. Kembali ke jendela utama QGIS, Anda akan melihat lapisan baris baru bernama earthquakes_with_nearest_citydimuat di panel Lapisan . Lapisan ini memiliki ciri-ciri garis yang menghubungkan setiap titik gempa dengan tempat berpenduduk terdekat. Pilih earthquakes_with_nearest_citylayer dan klik ikon Open Attribute Tabel di Toolbar .

14. Gulir ke kanan ke kolom terakhir, dan Anda akan melihat 2 atribut baru bernama HubName dan HubDist ditambahkan ke fitur gempa asli. Ini adalah nama jarak ke tetangga terdekat dari layer tempat berpenduduk.

============================================================================================

## Project 5 -- Sampling Raster Data using Points or Polygons

1. Buka zip dan ekstrak keduanya 2018_Gaz_ua_national.zipdan tl_2018_us_county.zipke folder di komputer Anda. Buka QGIS dan cari us.tmax_nohads_ll_20190501_float.tiffile di Peramban QGIS seret ke kanvas.

2. Anda akan melihat layer raster baru us.tmax_nohads_ll_20190501_floatdimuat di panel Layers . Lapisan raster ini berisi suhu maksimum yang tercatat pada setiap piksel dalam derajat Celcius. Selanjutnya kita akan memuat file titik area perkotaan. File ini hadir sebagai file teks dalam format Tab Separated Values ​​(TSV). Klik tombol Buka Pengelola Sumber Data di Bilah Alat Sumber Data

3. Beralih ke tab Teks Dibatasi . Klik tombol … di sebelah Nama file dan tentukan jalur ke file teks yang Anda unduh. Di bagian File format , pilih Custom delimiters dan centang Tab . Pilih INTPTLONGsebagai bidang X dan INTPTLATsebagai bidang Y . Klik Tambah lalu Tutup .

4. Lapisan titik baru 2018_Gaz_ua_nationalakan dimuat di panel Lapisan . Sekarang kita siap untuk mengekstrak nilai dari lapisan raster pada titik-titik ini. Pergi ke Memproses ‣ Kotak Alat

5. Cari dan temukan analisis Raster ‣ Contoh algoritme nilai raster. Klik dua kali untuk meluncurkannya.

6. Pilih 2018_Gaz_ua_nationalsebagai Layer Titik Input . Pilih us.tmax_nohads_ll_20190501_floatsebagai Lapisan Raster untuk sampel . Luaskan parameter Lanjutan dan masukkan tmaxsebagai awalan kolom Keluaran . Klik Jalankan . Setelah pemrosesan selesai, klik Tutup .

7. Lapisan baru akan dimuat di panel Lapisan . Pilih alat Identifikasi di Bilah Alat Atribut dan klik titik mana pun. Anda akan melihat atribut ditampilkan di panel Identifikasi Hasil . Anda akan melihat atribut baru bernama tmax_1 ditambahkan ke setiap fitur. Ini adalah nilai piksel dari lapisan raster yang diekstraksi di lokasi titik. Angka 1 mewakili nomor pita raster. Jika lapisan raster memiliki banyak pita, Anda akan melihat banyak kolom baru di lapisan keluaran.Sampled Points

8. Bagian pertama dari analisis kami selesai. Mari kita hapus lapisan yang tidak perlu. Tahan Shifttombol dan pilih dan lapisan. Klik kanan dan pilih Hapus untuk menghapusnya dari QGIS. Saat diminta untuk Hapus 2 entri legenda? , pilih Oke .Sampled Points2018_Gaz_ua_national

9. Sekarang kita akan menggunakan lapisan kabupaten untuk mengambil sampel raster dan menghitung suhu rata-rata untuk setiap kabupaten. Temukan tl_2018_us_county.shpfile di Peramban QGIS seret ke kanvas.

----------------------------------

Catatan

Sebagian besar algoritme pemrosesan akan membaca lapisan masukan dan membuat lapisan baru. Tetapi algoritma Statistik Zona berbeda. Itu memodifikasi lapisan input dan menambahkan atribut baru ke dalamnya. Itulah mengapa penting untuk meng-unzip file input terlebih dahulu. QGIS dapat memuat lapisan dari arsip zip secara langsung, tetapi tidak dapat memodifikasi lapisan yang di-zip. Algoritme pemrosesan akan gagal jika tidak dapat memperbarui lapisan masukan

---------------------------------

10. Lapisan baru tl_2018_us_countyakan dimuat ke panel Lapisan . Pergi ke Memproses ‣ Kotak Alat .

11. Cari dan temukan analisis Raster ‣ Algoritme statistik zona dan klik dua kali untuk meluncurkannya.

12. Pilih us.tmax_nohads_ll_20190501_floatsebagai layer Raster dan tl_2018_us_countysebagai layer Vector yang mengandung zones . Masukkan tmax_sebagai awalan kolom Keluaran . Klik … di sebelah Statistik untuk menghitung .

13. Pilih hanya Meannilainya dan klik OK .

14. Klik Jalankan untuk memulai pemrosesan. Algoritme mungkin membutuhkan waktu beberapa menit untuk selesai. Klik Tutup .

15. Seperti disebutkan sebelumnya, algoritma Zonal Statistics tidak membuat layer baru, tetapi memodifikasi layer zona. Klik kanan tl_2018_us_countylayer, dan pilih Open Attribute Table .

16. Anda akan melihat kolom baru bernama tmax_meanditambahkan ke tabel atribut. Ini berisi nilai suhu rata-rata yang diekstraksi di atas poligon untuk setiap fitur. Ada beberapa nilai nol karena kabupaten tersebut (milik Alaska, Hawaii, dan Puerto Riko) berada di luar jangkauan lapisan raster.

============================================================================================

## Project 6 -- Calculating Raster Area

1. Buka zip semua file yang diunduh. Di Browser , cari folder yang berisi file batas WDPA_WDOECM_Apr2022_Publicc_10744_shp-polygons.shpdan seret dan lepas ke kanvas QGIS.

2. Sekarang cari ubin raster penutup dunia ESA_WorldCover_10m_2020_v100_N24_E093_Map.tifdan jatuhkan ke kanvas QGIS.

3. Anda sekarang akan memiliki layer vektor batas dan raster tutupan lahan dimuat di panel Layers . Mari potong raster tutupan lahan ke batas taman nasional. Pergi ke Processing ‣ Toolbox untuk membuka Processing toolbox. Cari dan temukan GDAL ‣ Ekstraksi raster ‣ Klip raster dengan algoritma lapisan topeng. Klik dua kali untuk meluncurkannya.

4. Dalam dialog Clip Raster by Mask Layer , pilih ESA_WorldCover_10m_2020_v100_N24_E093_Maplayer sebagai Input layer dan WDPA_WDOECM_Apr2022_Publicc_10744_shp-polygonslayer sebagai Mask Layer . Masukkan -9999di Tetapkan nilai nodata yang ditentukan ke bagian pita keluaran.

5. Sekarang buka bagian Advanced Parameters dan pilih di Profile . Sekarang di bawah Clipped (mask) , klik dan pilih Save To File… . Masukkan nama file sebagai . Klik Jalankan .High Compression...worldcover_clipped.tif

6. Sekarang worldcover_clippedlayer akan dimuat di kanvas QGIS. Klik kanan ESA_WorldCover_10m_2020_v100_N24_E093_Maplayer dan pilih Hapus Lapisan…

7. Kedua layer kami masuk dalam CRS Geografis EPSG:4326. CRS ini memiliki satuan derajat dan tidak cocok untuk menghitung luas. Pertama-tama kita harus memproyeksikan ulang layer ke Projected CRS. untuk analisis regional seperti ini, UTM adalah pilihan yang baik untuk proyeksi CRS. Kami akan memproyeksikan ulang layer ke CRS untuk zona UTM lokal. Buka kotak alat Pemrosesan dan cari Vector general ‣ Algoritma lapisan proyeksi ulang . Klik dua kali untuk meluncurkannya.

8. Dalam dialog Reproject Layer , pilih WDPA_WDOECM_Apr2022_Publicc_10744_shp-polygonslayer sebagai Input layer , klik tombol Select CRS di bawah Target CRS .

9. Area minat kami termasuk dalam Zona UTM 46N. Cari 46 N dan pilih CRS.WGS 84 / UTM zone 46N

-----------------------------

Catatan

Untuk mengetahui zona UTM mana untuk wilayah Anda, lihat situs web What UTM Zone am I.

------------------------------

10. Di bagian Diproyeksikan ulang , klik ...dan pilih Simpan Ke File… . Masukkan nama sebagai nationalpark_reprojected.gpkg. Klik Jalankan .

11. Sekarang nationalpark_reprojectedlayer akan dimuat di kanvas. Klik kanan WDPA_WDOECM_Apr2022_Publicc_10744_shp-polygonslayer dan pilih Remove Layer… untuk menghapusnya. Sekarang kita akan memproyeksikan ulang layer raster. Di Kotak Alat Pemrosesan , cari dan temukan GDAL ‣ Proyeksi raster ‣ Warp (proyeksi ulang)

12. Dalam dialog Warp (Reproject) pilih worldcover_clippedsebagai Input layer , pilih CRS di Target CRS . Buka Advanced Parameters dan pilih di Profile .WGS 84 / UTM zone 46NHigh Compression

13. Sekarang di bawah Diproyeksikan ulang , klik ...dan pilih Simpan Ke File… . Masukkan nama sebagai worldcover_reprojected.tif. Klik Jalankan .

14. Sekarang worldcover_reprojectedlayer akan dimuat di kanvas, hapus worldcover_clippedlayer tersebut. Mari atur layer proyek ke zona UTM. Klik pada layer mana saja dan pilih Layer CRS ‣ Set ​​Project CRS from Layer .

15. Sekarang proyek CRS akan diperbarui. Mari atur simbologi lapisan raster sesuai nama kelas dan warna kumpulan data ESA WorldCover. Klik kanan pada worldcover_reprojectedlayer dan klik Properties…

16. Dalam dialog Layer Properties , pilih Symbology . Anda dapat melihat warna Layer divisualisasikan dalam nada putih-hitam. Untuk memperbaikinya, klik Style ‣ Load Style… . Telusuri dan pilih ESAWorldCover_ColorLegend.qmlfile.

17. Sekarang Anda dapat melihat warna simbol yang diperbarui dan deskripsi kelas. Klik Oke .

18. Perluas worldcover_reprojectedlayer di panel Layers untuk melihat legenda dengan deskripsi kelas yang benar.

19. Sekarang mari kita hitung luas untuk setiap kelas. Di kotak alat pemrosesan, cari dan temukan alat laporan nilai unik lapisan Raster . Klik dua kali untuk membukanya.

20. Dalam dialog Raster Layer Unique Values ​​Report , pilih Input layer sebagai worldcover_reprojected. Di bawah tabel Unique values ​​klik ...dan pilih Save to File… . Masukkan nama sebagai class_areas.gpkg. Klik Jalankan .

21. Sekarang class_areaslayer akan ditambahkan ke panel Layers . Klik kanan pada layer dan klik Open Attribute Table . Kolom m2berisi luas untuk setiap kelas dalam meter persegi.

22. Mari ubah luas menjadi kilometer persegi. Di Processing Toolbox , cari dan pilih Vector table ‣ Field Calculator .

23. Dalam dialog Kalkulator Bidangclass_areas , pilih lapisan di Lapisan Input . Masukkan nama Bidang sebagai area_sqkm. Di bidang Hasil, pilih Float. Di jendela Ekspresi , masukkan ekspresi di bawah ini. Ini akan mengubah sqmt menjadi sqkm dan membulatkan hasilnya menjadi 2 tempat desimal. Di bawah Terhitung klik ...dan pilih Simpan Ke File… . Masukkan nama sebagai class_area_sqkm.gpkg. Klik Jalankan .

-------------------

round("m2"/ 1e6, 2)

------------------

24. Sekarang class_area_sqkmlayer akan dimuat di kanvas. Buka tabel Atribut dan periksa kolom area_sqkm yang baru ditambahkan . Anda akan melihat bahwa kolom Nilai berisi angka untuk setiap kelas. Agar hasilnya lebih mudah diinterpretasikan, Mari tambahkan juga deskripsi untuk setiap nomor kelas. Deskripsi kelas tersedia di Panduan Pengguna Produk ESA .

25. Buka Field Calculator, dan pilih class_areas_sqkmlayer di Input Layer . Masukkan nama Bidang sebagai landcover, dalam jenis bidang Hasil , pilih String. Di jendela Expression masukkan ekspresi di bawah ini. Ekspresi ini menggunakan pernyataan CASE untuk menetapkan nilai berdasarkan beberapa kondisi. Di bawah Terhitung klik ...dan pilih Simpan Ke File… . Masukkan nama sebagai class_area_with_landcover.gpkg. Klik Jalankan .

------------------------

CASE
WHEN "value" = 10 THEN 'Tree cover'
WHEN "value" = 20 THEN 'Shrubland'
WHEN "value" = 30 THEN 'Grassland'
WHEN "value" = 40 THEN 'Cropland'
WHEN "value" = 50 THEN 'Built-up'
WHEN "value" = 60 THEN 'Bare / sparse vegetation'
WHEN "value" = 70 THEN 'Snow and Ice'
WHEN "value" = 80 THEN 'Permanent water bodies'
WHEN "value" = 90 THEN 'Herbaceous wetland'
WHEN "value" = 95 THEN 'Moss and lichen'
WHEN "value" = 100 THEN 'Mangroves'
END

-----------------------------------------

26. Sekarang class_area_with_landcoverlayer akan dimuat di kanvas. Buka tabel Atribut. Kolom tutupan lahan akan berisi nama tutupan lahan terhadap setiap nilai tutupan lahan.

27. Mari ekspor hasil ini sebagai file excel. Sebelum mengekspor, kami juga akan mengatur tabel dan menghapus kolom yang tidak diinginkan. Di Processing Toolbox , cari dan pilih Vector table ‣ Refactor fields .

28. Dalam dialog Refactor Fieldsclass_area_with_landcover , pilih layer di Input Layer . Pilih semua kolom kecuali area_sqkm dan landcover , lalu klik Delete selected field .

29. Anda juga dapat mengubah urutan bidang dalam tabel menggunakan tombol Pindahkan Bidang yang Dipilih . Setelah Anda selesai mengedit, klik ...tombol di sebelah Refactored dan pilih Save To File… . Pilih sebagai format. Masukkan nama file sebagai dan klik Simpan . Dalam dialog Bidang Refaktor, klik Jalankan untuk menerapkan perubahan Anda.XLSX Files (*.xlsx)park_area_by_landcover.xlsx

30. Hasilnya akan berupa spreadsheet dengan kolom landcover dan area_sqkm .

============================================================================================

## Project 7 -- Creating Heatmaps

1. Pertama-tama kita akan memuat layer peta dasar dari OpenStreetMap dan kemudian mengimpor data CSV. Di tab Browser , gulir ke bawah dan temukan bagian Ubin XYZ .

2. Bentangkan untuk melihat layer petak OpenStreetMap . Seret dan lepas ke kanvas utama. Selanjutnya kita akan memuat file CSV. Klik tombol Buka Pengelola Sumber Data 

3. Beralih ke tab Teks Dibatasi . Di sini kita akan mengimpor data kejahatan yang datang dalam file teks format CSV. Klik tombol … di sebelah Nama file dan ramban ke 2019-02-surrey-street.csvfile yang diunduh. Bidang X dan bidang Y di bagian Definisi Geometri akan diisi secara otomatis dengan kolom Longitudedan Latitude. Geometri CRS harus dibiarkan definisi default . Pastikan data terlihat benar di panel Sample data dan klik Add , diikuti oleh Close .EPSG:4326 - WGS 84

4. Anda akan melihat 2 lapisan - OpenStreetMapdan dimuat di panel Lapisan2019-02-surrey-street QGIS . Klik kanan layer dan pilih Zoom to Layer .2019-02-surrey-street

5. Anda akan melihat layer poin insiden kejahatan dihamparkan pada peta dasar OpenStreetMap. Zoom dan Pan untuk mengeksplorasi data. Datanya cukup padat dan sulit untuk mengetahui di mana terdapat konsentrasi kejahatan yang tinggi. Di sinilah visualisasi peta panas akan berguna. Pilih 2019-02-surrey-streetlayer dan klik tombol panel Open the Layer Styling .

6. Pilih Heatmapsebagai penyaji di menu dropbox. Panel Layer Styling bersifat interaktif dan Anda dapat segera melihat efek perubahan Anda tercermin di kanvas. Lapisan sekarang akan ditampilkan di jalan warna skala abu-abu default.

7. Peta panas biasanya dirender menggunakan jalur warna kuning ke merah atau putih ke merah di mana konsentrasi titik yang lebih tinggi menghasilkan lebih banyak panas . Klik menu dropdown Color ramp dan pilih Redscolor-ramp.

8. Selanjutnya Anda harus memilih Radius . Parameter ini menentukan lingkungan melingkar di sekitar setiap titik di mana titik tersebut akan memiliki pengaruh. Nilai ini akan sangat bergantung pada jenis data masukan Anda. Untuk data kami, anggap saja insiden kejahatan akan berdampak hingga 5 Kilometer dari lokasi. Perhatikan bahwa CRS proyek saat ini diatur ke sudut kanan bawah. CRS ini memiliki satuan meter, jadi kita harus menentukan meter sebagai radius. Parameter lain yang disembunyikan dari menu ini adalah bentuk Kernel . Ini adalah fungsi yang menentukan bagaimana pengaruh suatu titik harus tersebar pada radius yang diberikan. Perender Heatmap menggunakan fungsi untuk perhitungan ini. Ada jenis kernel lain seperti ,EPSG: 38575000QuarticTriangularUniform, Triweightdan Epanechnikovitu bisa ditentukan saat menggunakan metode pembuatan peta panas berbeda yang dijelaskan nanti di tutorial ini. Lihat posting ini untuk penjelasan dan panduan yang bagus untuk memilih radius dan bentuk kernel yang tepat.

9. Visualisasi peta panas sudah siap. Kita bisa mengatur Opacity dari heatmap di bagian Layer Rendering di bagian bawah. Atur opacity agar Anda dapat melihat peta dasar beserta peta panasnya.60 %

10. Untuk banyak jenis analisis, hanya mempertimbangkan kerapatan poin sudah cukup baik. Namun terkadang, Anda mungkin ingin memberikan kepentingan yang berbeda untuk setiap poin. Kejahatan yang lebih kejam seharusnya memiliki pengaruh lebih besar pada peta panas keluaran daripada perampokan. Demikian pula, kadang-kadang suatu titik dapat mewakili beberapa pengamatan di satu lokasi yang perlu diperhitungkan dalam analisis. Untuk melakukannya, Anda dapat menyediakan kolom bobot numerik opsional yang menentukan nilai untuk setiap poin. Mari tambahkan bidang bobot dan gunakan untuk menyempurnakan peta panas. Klik kanan 2019-02-surrey-streetlayer dan pilih Open Attribute Table .

11. Anda akan melihat bidang teks yang disebut dalam data masukan yang menjelaskan jenis kejahatan. Kita dapat menggunakan ini untuk mengkategorikan berbagai jenis kejahatan dan menetapkan bobot yang lebih tinggi untuk kejahatan yang lebih kejam.Crime type

12. Klik Kalkulator lapangan terbuka .

13. Kami sekarang akan memasukkan rumus yang menggunakan dan menentukan nilai bobot. QGIS memiliki cara praktis untuk menambahkan bidang yang dihitung tersebut menggunakan Bidang Virtual . Bidang virtual disimpan dalam proyek QGIS dan tidak mengubah data sumber. Ini juga dihitung secara dinamis dan dapat digunakan di mana saja di QGIS seperti halnya nilai atribut lainnya. Masukkan sebagai nama bidang Keluaran dan setel jenis bidang Keluaran ke . Masukkan ekspresi berikut di editor Ekspresi . Di sini kita menggunakan pernyataan CASE untuk menetapkan nilai yang berbeda berdasarkan kondisi yang berbeda. Klik Oke .Crime typeweightWhole number (integer)

--------------------

CASE
WHEN "Crime type" LIKE 'Violence%' THEN 10
WHEN "Crime type" LIKE 'Criminal%' THEN 5
ELSE 1
END

-------------------

14. Atribut baru akan ditambahkan untuk setiap fitur dengan nilai bobot yang sesuai.

15. Kembali ke panel Layer Styling , klik menu drop-down untuk Weight points by dan pilih bidang yang baru ditambahkan weight.

16. Anda akan melihat perubahan rendering peta panas untuk memperhitungkan parameter bobot. Tutup panel Layer Styling .

17. Jika Anda memerlukan visualisasi peta panas untuk disimpan sebagai lapisan raster permanen atau ingin menyesuaikan peta panas dengan opsi lanjutan seperti kernel yang berbeda atau radius dinamis, Anda dapat menggunakan Peta Panas (Estimasi Kepadatan Kernel) dari Kotak Alat Pemrosesan. Kami sekarang akan menggunakan algoritma ini. Pergi ke Memproses ‣ Kotak Alat .

18. Sebelum kita dapat membuat peta panas, kita perlu memproyeksikan ulang data sumber ke CRS yang diproyeksikan. Karena jarak memainkan peran penting dalam perhitungan peta panas, tidak benar menggunakan CRS geografis. Cari dan temukan algoritma Vector general ‣ Proyeksi ulang layer .

19. Pada dialog Reproject layer , klik tombol Select CRS untuk Target CRS . Cari dan pilih CRS. CRS yang diproyeksikan ini adalah pilihan yang baik untuk data di Inggris. Klik Jalankan .EPSG:27700 OSGB 1936 / British National Grid

20. Layer baru bernama Reprojectedakan ditambahkan ke panel Layers . Hapus centang pada kotak di sebelah 2019-02-surrey-streetlapisan lama untuk menyembunyikannya.

21. Cari dan temukan algoritma Interpolation ‣ Heatmap (Kernel Density Estimation) .

22. Pada dialog Heatmap (Kernel Density Estimation) , kita akan menggunakan parameter yang sama seperti sebelumnya. Pilih Radius sebagai 5000meter dan Berat dari bidang sebagai weight. Atur ukuran Piksel X dan Ukuran Piksel Y menjadi 50meter. Biarkan Kernel membentuk nilai default Quartic. Klik Jalankan .

--------------------

Catatan

Parameter Radius from field memungkinkan Anda menentukan radius pencarian dinamis untuk setiap titik. Ini dapat digunakan bersama dengan Bobot dari bidang untuk memiliki kontrol yang lebih baik tentang bagaimana pengaruh setiap titik tersebar.

---------------------------

23. Setelah pemrosesan selesai, lapisan raster baru bernama OUTPUTakan dimuat. Visualisasi default jelek karena menggunakan perender. Klik tombol panel Open the Layer Styling .Singleband gray

24. Ubah render menjadi dan pilih ramp warna. Lapisan sekarang terlihat seperti visualisasi peta panas yang telah kita buat sebelumnya.Singleband PseudocolorReds

-------------------------------

Catatan

Perhatikan bahwa OUTPUTlayer pada panel Layers memiliki legenda tetapi 2019-02-surrey-streetlayer tidak. Masalah umum saat menggunakan lapisan peta panas yang dibuat dengan perender Peta Panas adalah kurangnya legenda. Katakanlah Anda ingin menggunakan peta panas di Tata Letak Cetak dan menambahkan legenda. Peta panas raster yang dibuat dengan metode algoritme pemrosesan Peta Panas memungkinkan hal ini.

----------------------------------

============================================================================================

## Project 8 -- Animating Time Series Data

1. Di Panel Peramban QGIS, temukan direktori tempat Anda menyimpan data unduhan. Luaskan ne_10m_land.zipdan pilih ne_10m_land.shplayer. Seret layer ke kanvas. Selanjutnya, cari ASAM_shp.zipfile. Perluas dan pilih asam_data_download/ASAM_events.shplayer dan seret ke kanvas.

2. Setelah lapisan dimuat, Anda dapat melihat masing-masing titik yang mewakili insiden lokasi pembajakan. Ada ribuan insiden dan sulit ditentukan dengan lebih banyak pembajakan. Daripada poin individual, cara yang lebih baik untuk memvisualisasikan data ini adalah melalui peta panas. Pilih ASAM_eventslayer dan klik tombol Open the layer Styling Panel di panel Layers . Klik tarik-turun.Single symbol

3. Di drop-down pemilihan penyaji, pilih Heatmappenyaji. Selanjutnya, pilih jalur Viridiswarna dari pemilih Jalur warna .

4. Sesuaikan nilai Radius ke 5.0. Di bagian bawah, luaskan bagian Layer Rendering dan sesuaikan Opacity menjadi 75.0%. Ini memberikan efek visual yang bagus dari hotspot dengan lapisan tanah di bawahnya.

5. Sekarang mari kita animasikan data ini untuk menunjukkan peta insiden pembajakan tahunan. Klik kanan pada ASAM_eventlayer, dan pilih Properties.

6. Di kotak dialog Properti lapisan , pilih tab Temporal dan aktifkan dengan mengklik kotak centang..

7. Data sumber berisi atribut dateofocc- yang mewakili tanggal terjadinya insiden. Ini adalah bidang yang akan digunakan untuk menentukan poin yang diberikan untuk setiap periode waktu. Pilih di menu Drop down Konfigurasi , sebagai Field .Single Field with Data/Timedateofocc

8. Sekarang simbol jam akan muncul di sebelah nama layer. Klik pada (ikon Jam) dari Bilah Alat Navigasi Peta.Temporal Control Panel

9. Klik pada (ikon putar) untuk mengaktifkan kontrol animasi. Klik Atur ke Rentang Penuh (ikon segarkan) di sebelah Rentang untuk secara otomatis mengatur rentang waktu agar cocok dengan kumpulan data.Animated Temporal Navigation

10. Sekarang Anda siap untuk melihat animasi. Atur Langkah lalu klik tombol Play untuk memulai animasi.1 Year

------------------------

Catatan

Jika animasi terlalu cepat, Anda dapat menyesuaikan frame rate dengan mengklik (ikon gerigi kuning) di pojok kanan atas panel Temporal Controller. Menurunkan frame rate (frame per detik) akan memperlambat animasi.Temporal Settings

-------------------

11. Akan sangat membantu juga untuk menampilkan label yang menunjukkan kerangka waktu saat ini di peta. Kita dapat melakukannya dengan menggunakan dekorasi Judul bawaan. Pergi ke Lihat ‣ Dekorasi ‣ Label Judul .

12. Klik kotak centang untuk mengaktifkannya dan klik tombol dan masukkan ekspresi berikut untuk menampilkan tahun. Di sini variabel berisi stempel waktu irisan waktu saat ini yang ditampilkan. Jadi kita bisa menggunakan stempel waktu itu dan memformatnya untuk menampilkan tahun kejadian. Lihat Dokumentasi QGIS untuk detail tentang berbagai opsi pemformatan yang didukung untuk stempel waktu.Insert an Expression@map_start_time

------------------

format_date(@map_start_time, 'yyyy')

------------------

13. Pilih ukuran font sebagai 25, atur warna bilah latar belakang sebagai Whitedan atur transparansi menjadi 50%. Di Penempatan pilih . Sekarang klik Oke.Bottom Right

14. Setelah parameter diatur sesuai, tahun akan ditampilkan seperti yang ditunjukkan. Untuk mengekspornya sebagai gambar dan mengonversinya sebagai GIF, pilih (ikon simpan) di jendela kontrol Temporal.Export Animation

15. Klik pada ... direktori Output untuk memilih direktori tempat gambar akan disimpan.

16. Di bawah Extent pilih Hitung dari Layer ‣ ne_10_land layer. Klik Simpan.

17. Setelah ekspor selesai, Anda akan melihat gambar PNG untuk setiap tahun (total 18 gambar) di direktori keluaran.

18. Sekarang mari buat GIF animasi dari gambar-gambar ini. Ada banyak opsi untuk membuat animasi dari masing-masing bingkai gambar. Saya suka ezgif untuk alat yang mudah dan online. Kunjungi situs dan klik Pilih File dan pilih semua file .png. Setelah dipilih, klik Unggah dan buat GIF! tombol. Setelah dibuat, Anda dapat mengunduh GIF menggunakan tombol Simpan .

============================================================================================

## Project 9 -- Handling Invalid Geometries

1. Jelajahi India-States.zipfile yang diunduh di Peramban QGIS. Perluas dan seret India-States.shpfile ke kanvas peta.

2. Anda akan melihat India-Stateslayer baru dimuat di panel Layers . Pergi ke Memproses ‣ Kotak Alat .

3. Kami akan mencoba menjalankan algoritme pemrosesan pada lapisan input untuk mendemonstrasikan bagaimana geometri yang tidak valid dapat menyebabkan masalah selama operasi geoproses. Cari dan temukan Kartografi ‣ Algoritma pewarnaan topologi. Klik dua kali untuk meluncurkannya.

4. Dalam dialog pewarnaan Topologi , pilih India-Statessebagai lapisan Input . Simpan semua parameter lain ke default dan klik Run .

------------------

Catatan

Algoritma pewarnaan topologi mengimplementasikan algoritma untuk mewarnai peta sehingga tidak ada poligon yang berdekatan memiliki warna yang sama. Ini adalah teknik kartografi yang berguna dan Teorema Empat Warna menyatakan bahwa 4 warna cukup untuk mencapai hasil ini. Ada versi teori graf dari torem ini yang disebut teorema Lima warna . Implementasi algoritma QGIS didasarkan pada grafik sehingga dalam praktiknya Anda akan melihat bahwa lapisan poligon kompleks seperti ini akan membutuhkan hingga 5 warna.

---------------------

5. Saat algoritme berjalan, Anda akan melihat peringatan ditampilkan di tab Log . 1 fitur di lapisan input memiliki geometri yang tidak valid dan dilewati selama pemrosesan. Pengaturan default untuk menangani geometri yang tidak valid di Kotak Alat Pemrosesan terletak di Pengaturan ‣ Opsi ‣ Pemrosesan ‣ Umum ‣ Pemfilteran fitur tidak valid dan diatur ke . Ini adalah pengaturan default yang bagus, tetapi jika masukan Anda besar, Anda mungkin melewatkan peringatan ini dan mungkin tidak mengetahui bahwa fitur masukan telah dilewati. Anda mungkin ingin mengubah nilainya menjadi .Skip (ignore) features with invalid geometriesStop algorithm execution when a geometry is invalid

6. Kembali ke jendela utama QGIS, Anda akan melihat layer baru Coloredditambahkan ke panel Layers . Perhatikan bahwa layer baru tidak memiliki status yang geometrinya tidak valid. Kami sekarang tahu bahwa poligon keadaan tertentu ini memiliki geometri yang tidak valid tetapi kami tidak tahu apa penyebabnya. Kita dapat dengan mudah mengetahuinya. Cari dan temukan geometri Vektor ‣ Periksa algoritme validitas.

7. Dalam dialog Check ValidityIndia-States , pilih sebagai Input layer . Pilih GEOSsebagai Metode . Klik Jalankan .

8. Saat algoritme selesai diproses, Anda akan melihat 3 layer baru di panel Layers - , dan . Lapisan berisi lokasi dan deskripsi kesalahan geometri. Klik kanan dan pilih Open Attribute Table .Valid outputInvalid outputError outputError output

------------------

Catatan

Dokumentasi QGIS memiliki artikel terperinci tentang Jenis pesan kesalahan dan artinya yang menjelaskan penyebab semua kesalahan.

------------------

9. Anda akan melihat bahwa pesan kesalahannya adalah Ring self-intersection . Pilih baris dan klik tombol Zoom map to selected features . Saat memperbesar, Anda akan melihat akar penyebab galat geometri.

10. QGIS hadir dengan algoritme bawaan untuk memperbaiki kesalahan geometri secara otomatis. Cari dan temukan geometri Vektor ‣ Perbaiki algoritma geometri . Klik dua kali untuk menjalankannya.

11. Dalam dialog Fix GeometriesIndia-States , pilih sebagai Input layer dan klik Run .

12. Layer baru akan ditambahkan ke panel Layers . Pada titik ini, kesalahan geometri telah diperbaiki dan Anda dapat menjalankan algoritme pemrosesan apa pun pada lapisan ini tanpa masalah. Tetapi kita dapat melihat bahwa masih ada celah antara poligon yang berdekatan yang tidak terduga dan dapat menyebabkan kesalahan topologi di kemudian hari. Kami juga dapat memperbaikinya dengan mengedit poligon. Klik tombol Toggle Editing pada Digitizing Toolbar . Pilih Vertex Tool dan dari drop-down pilih .Fixed GeometriesVertex Tool (Current Layer)

13. Saat alat vertex aktif, klik pada vertex untuk memilihnya. Anda dapat menekan Deletetombol untuk menghapus simpul atau menyeretnya untuk memindahkannya. Anda dapat memindahkan simpul sehingga tepi poligon sekarang menyentuh poligon yang berdekatan.

14. Setelah selesai, klik tombol Toggle Editing lagi dan klik Save .

15. Mari jalankan lagi algoritma Kartografi ‣ Pewarnaan topologi .

16. Dalam dialog Pewarnaan Topologi , pastikan Anda memilih sebagai lapisan Input . Klik Jalankan .Fixed Geometries

17. Anda akan melihat algoritme berjalan tanpa kesalahan dan lapisan baru Coloredakan ditambahkan ke panel Lapisan . Perhatikan bahwa algoritme tidak mewarnai lapisan dengan sendirinya, tetapi bekerja dengan menambahkan kolom baru yang dipanggil color_idke setiap poligon yang dapat digunakan untuk menetapkan warna unik yang berbeda dari poligon yang berdekatan. Pilih Coloredlayer dan klik tombol Open the Layer Styling Panel .

18. Pilih Categorizedpenyaji dan kolom color_idsebagai Value . Klik Klasifikasikan . Anda sekarang akan melihat peta berwarna sehingga poligon yang berdekatan memiliki warna yang berbeda.

