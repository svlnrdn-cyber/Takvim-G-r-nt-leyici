<pre>
#include &lt;stdio.h&gt;

// Artık yıl olup olmadığını kontrol eder
int artik_yil_mi(int yil)
{
        //Yıl 4 e tam bölünmeli ama 100e tam bölünmemeli
        if (yil % 4 == 0 && yil % 100 != 0)
    {
        return 1;
    }
        else if (yil % 400 == 0) // ya da 400e tam bölünmeli
    {
        return 1; // Artık yıl
    }
        else
    {
        return 0; // Artık yıl değil
    }
}
//Hangi ayın kaç çektiği
int ay_gun_sayisi(int ay, int yil)
{
         //30 çeken aylar
         if (ay == 4 || ay == 6 || ay == 9 || ay == 11)
    {
         return 30;
         //şubatın artık yılsa 29 değilse 28 çekmesi için
    }
         else if (ay == 2)
    {
         if (artik_yil_mi(yil))
    {
         return 29;
    }
         else
    {
         return 28;
    }
    }
         else //kalan diğer ayların 31 çekmesi
    {
         return 31;
    }
}
//ayın 1'inin hangi güne geldiğini bulur.
int ilk_gun_hafta_kac(int ay, int yil)
{

         int gunler = 0;
         int i;
         //kullanıcının istediği aydan önceki tüm aylardaki total gün sayılarını toplar.
         for (i = 1; i < ay; i++)
    {
         gunler += ay_gun_sayisi(i, yil);
    }

         //yılın içinde gün sayımı yapar ve artık yıl mı kontrolü yaparak 366 veya 365 ekler
         for (i = 1900; i < yil; i++)
    {
         gunler += (artik_yil_mi(i) ? 366 : 365);
    }
         /*
         bu günlerin toplamına 1 eklenir ve 7 ye bölünür,
         elde edilen kalan ile ayın hangi gün başlanacağı bulunur.
         0 = pazartesi, 6 = pazar (0,6)
         */
        return (gunler + 1) % 7;
}
       //(1,,12) ay numaralarını ayların isimlerine dönüştürür.
void takvim_goster(int ay, int yil)
{
        const char *ay_isimleri[] = {"", "Ocak", "Şubat", "Mart", "Nisan", "Mayıs", "Haziran",
                                "Temmuz", "Ağustos", "Eylül", "Ekim", "Kasım", "Aralık"};

        //günleri (0,6) arası numaralandırır.
        int hafta_basi = ilk_gun_hafta_kac(ay, yil);
        //sutunda düzgün görünmesi için
        int baslangic_gunu = (hafta_basi == 0) ? 6 : (hafta_basi - 1);
        int gun_sayisi = ay_gun_sayisi(ay, yil);
        int mevcut_gun = 1;

        // Ay, yıl ve günleri başlıklar halinde yazar.
        printf("\n\n=== %s %d Takvimi ===\n", ay_isimleri[ay], yil);
        printf("Pzt Sal Car Per Cum Cmt Paz\n");

        // Ayın 1. gününden önceki haftanın günleri kadar boşluk bırakır.
        for (int i = 0; i < baslangic_gunu; i++) {
        printf("    ");
    }

        // ilk günden son güne (istenilen ay kaç çekiyorsa) yazdıran fonksiyon.
        while (mevcut_gun <= gun_sayisi)
    {
        printf("%3d ", mevcut_gun);

        // yazdırılan günün pazar a denk gelip gelmediğini kontrol eden,
        // eğer pazar ise  alt satıra geçmesini sağlayan fonksiyon.
        if ((baslangic_gunu + mevcut_gun) % 7 == 0)
    {
        printf("\n");
    }

        mevcut_gun++;
    }

        printf("\n");
}


int main()
{
        int yil, ay;
        while (1)
    {
        //kullanıcıdan yıl ay bilgisi alınır.
        printf("Takvimini gormek istediğiniz yılı girin: ");
        //hata durumu, geçersiz karakter, yanlış karakter girişi durumunda
        if (scanf("%d", &yil) != 1)
    {
         while (getchar() != '\n'); //temizle
         printf("Hatalı giriş. \n");
         continue;
    }
         printf(" 1 = OCAK, 2 = ŞUBAT ... 12 = ARALIK. \n");
         printf("Takvimini görmek istediğiniz ayın numarasın girin (1-12 araı): ");
        if (scanf("%d", &ay) != 1 )
    {
         while(getchar() != '\n');
        printf("Hatalı giriş. \n");
        continue;
    }

         // yanıtın geçerliliği kontrol edilir (girilen ay, 1-12 arasında olmalı ve yıl, en az 1900 olmalı).
         if (ay < 1 || ay > 12 || yil < 1900)
    {
         printf("Hatalı giriş. Geçersiz ay veya yil (min 1900) girdiniz.\n");
         continue;
    }
        //takvimi ekrana yazdıran fonksiyon
         takvim_goster(ay, yil);
    }
}
</pre>
