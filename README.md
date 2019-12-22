# mergeSortC
c merge sort 

#include <stdio.h>
#include <stdlib.h>
#define BOYUT 10

//mainden önce tanımladık ki mainde bu fonksiyonları çağırabilelim.
void mergeSort(int*,int,int);
void merge(int*,int,int,int);


int main()
{
//dizimizi oluşturuyoruz.
int girilenSayilar[BOYUT];

FILE *fptr;
  
  // dosyaya yazma
  fptr = fopen("file.txt","w");
  if(fptr == NULL){
    printf("Hata!");   
    exit(1);             
  } 
  fprintf(fptr,"Dizinizin Sırasız Hali: ");
  for(int i=0;i<BOYUT;i++){
    printf("Sayi Giriniz: ");
    scanf("%d",&girilenSayilar[i]);
    fprintf(fptr,"%d",girilenSayilar[i]);
    fputs(" ", fptr);

  }
  fputs(" " ,fptr);
  



//Sıralanmış diziyi dosyaya yazdırıyoruz.
mergeSort(girilenSayilar, 0, BOYUT - 1);
fprintf(fptr,"\nDizinizin Sıralı Hali: ");
for(int j=0;j<BOYUT;j++){
  fprintf(fptr,"%d",girilenSayilar[j]);
  fputs(" ",fptr);
}

fclose(fptr);
return 0;
}



/*                     {5,3,96,78,56,25,6,16,34,88}
1. adim        {5,3,96,78,56}               {25,6,16,34,88}    :<-- dizi ikiye bölündü.
2. adim      {5,3,96}    {78,56}        {25,6,16}      {34,88} 
3. adim     {5,3} {96}   {78} {56}     {25,6} {16}    {34} {88}
4. adim    {5} {3} {96}   {78} {56}   {25} {6} {16}   {34} {88} :<-- elemanlar tek tek ayrıldı.
5. adim      {3,5,96}     {56,78}       {6,16,25}      {34,88}  :<-- sıralayarak birleştiriliyor. 
6. adim         {3,5,56,78,96}              {6,16,25,34,88}                                   
7. adim                {3,5,6,16,25,34,56,78,88,96}         :<-- dizi birleştirildi.
*/



//mergeSort fonksiyonumuz diziyi tek elemanlı olana kadar böler.
void mergeSort(int sirali_dizi[], int sol1, int sag1)
{
if (sol1 < sag1)
{
//dizinin ortası hesaplanır.
int orta1 = sol1+(sag1-sol1)/2;

mergeSort(sirali_dizi, sol1, orta1); // sol diziyi böler.
mergeSort(sirali_dizi, orta1 + 1, sag1); // sağ diziyi böler.

//Esas fonksiyona dizimizi, sol indisi, orta indisi ve sağ indisi parametre olarak yolluyoruz
merge(sirali_dizi, sol1, orta1, sag1);
}


}


// merge fonksiyonu bölünen dizideki elemanları karşılaştırma yaparak sıralar.

void merge(int dizi[], int bas, int orta, int son)
{
int i, j, k;
//Sol ve sağ dizi için değişkenler

int a1 = orta - bas + 1;
int a2 = son - orta;

//Geçici dizilerimiz.

int sol[a1], sag[a2];

//for geçici dizilerimize elemanları atıyor.

for (i = 0; i < a1; i++)
sol[i] = dizi[bas + i];

for (j = 0; j < a2; j++)
sag[j] = dizi[orta + 1+ j];

//
i = 0;    //1. alt dizinin başlangıç indeksi
j = 0;    //2. alt dizinin başlangıç indeksi
k = bas;  //birleştirilmiş alt dizinin başlangıç indeksi

//Birleştirme 
//sol ve sağ dizide eleman oldukça while devam edicek.

while (i < a1 && j < a2)// sol kısım ortaya sağ kısım bitişe gelene kadar.
{

//Sol ve sağ dizinin elemanları karşılaştırılır. Eğer sol küçük eşitse yeni dizinin ilk elemanı olur.

if (sol[i] <= sag[j])
{
dizi[k] = sol[i];
i++;
}
//Değilse sağ dizinin ilk elemanı yeni dizimizin ilk elemanı olur.

else
{
dizi[k] = sag[j];
j++;
}

k++;//diğer indise geçilir.
}

//Eğer boşta eleman kaldıysa o elemanı atama işlemi yapıyor.

while (i < a1)
{
dizi[k] = sol[i];
i++;
k++;
}

//Eğer boşta eleman kaldıysa o elemanı atama işlemi yapıyor.

while (j < a2)
{
dizi[k] = sag[j];
j++;
k++;
}
}


