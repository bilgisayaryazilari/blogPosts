# Tek Yönlü Bağlı Listede İkili Arama

Bu yazının amacı tek yönlü bağlı listede ikili aramanın nasıl yapıldığını göstermektir. Bunun için arama yapılan bağlı listenin sıralı olması ve eleman sayısının belli olması gerekmektedir.

Öncelikle bilinmelidir ki sıralı bir bağlı liste üzerinde yapılan bir ikili arama, sıralı bir dizi üzerinde olduğu gibi O(log(n)) karmaşıklıkta değildir. Bunun nedeni, bağlı listelerde eleman erişiminin dizilerdeki gibi sabit zamanda (constant time) olmamasıdır. Buna rağmen sıralı bağlı listelerde de en hızlı arama algoritması ikili aramadır. Çünkü elemanlara n adımda ulaşılır ancak doğrusal aramanın aksine log(n) defa karşılaştırma yapılır.

### Algoritma

İkili aramayı gerçekleştirmek için bağlı liste üzerinde iki farklı pointer ile dolaşılır. Bu yazıda bunlar lowIter ve midIter şeklinde adlandırılmıştır. Başlangıçta ikisi de listenin başını gösterir.

1. Aramaya başlamak için midIter n / 2 adım ilerletilerek ortaya getirilir.
1. Oradaki elemanın aranan değer olup olmadığına bakılır.
1. Aranan değerse işlem tamamlanır.
1. Aranan değerden küçükse lowIter bir sonraki elemana atanır ve midIter'e lowIter atanır.
1. Aranan değerden büyükse midIter'e lowIter atanır.
1. n yarıya düşürülür ve 1. adıma gidilir.

### C Dilinde Kodlanmış Örnek

**Bağlı listenin oluşturulduğu struct**

```c
struct linkedlist {
    int data;
    struct linkedlist *next;
};
```

**Arama fonksiyonu**

```c
/**
  * @param x Aranan değer.
  * @param len Listenin eleman sayısı.
  * @param head Listenin başı.
  * @return Değer bulunursa listedeki indisi, bulunamazsa -1 döner.
  */
int bSearch(int x, int len, struct linkedlist *head) {
    int low = 0, high = len -1, mid = (low + high) / 2, i;
    struct linkedlist *lowIter = head, *midIter = head;
    for (i = low; i < mid; i++)
        midIter = midIter->next;
    while (low < high && midIter->data != x) {
        if (midIter->data < x) {
            low = mid + 1;
            lowIter = midIter->next;
            midIter = lowIter;
        } else {
            high = mid -1;
            midIter = lowIter;
        }
        mid = (low + high) / 2;
        for (i = low; i < mid; i++)
            midIter = midIter->next;
    }
    if (midIter->data == x)
        return mid;
    else
        return -1;
}
```
