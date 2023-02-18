---
layout: post
title: "Tam Bağımsız ve Tekil Sorumlu Try-Catch"
date: 2013-06-17 19:00
categories: [Turkish, Coding]
keywords: try catch, solid
author: Özgür Adem Işıklı
lang: tr
---

Try-Catch bloklarını kullanarak hata yakalama sürecine aşina olduğunuzu varsayıyorum. Eğer değilseniz [buradan](https://web.archive.org/web/20140906095623/http://php.net/manual/en/language.exceptions.php) temel bilgileri edinebilirsiniz. Yazı boyunca SOLID prensiplerinden Single Responsibility (Tek Sorumluluk) prensibine de değineceğim. Eğer bu konuda da ön bilginiz yoksa [buradan](https://web.archive.org/web/20140920213104/http://code.tutsplus.com/tutorials/solid-part-1-the-single-responsibility-principle--net-36074) faydalanabilirsiniz.

SOLID prensiplerini yazdığım her kod üzerinde istisnasız olarak uygulamak istiyorum. Bu nedenle epey dikkatli çalışıyorum. Özellikle metotlarımın tek sorumluluk prensibine göre yazıldığına önem gösteriyorum. Bu, bir çok işi yapan metotları parçalara bölmemi gerektiriyor. Ancak muhakkak bir noktadan sonra parçalara bölünen metotları çağırdığım bir ön metot olması gerekiyor. Bir kullanıcı kayıt işlemini ele alırsak genel olarak metotlar bu şekilde oluşuyor;

<pre><code class="language-php">
class UserInsertController
{
 
    public function provider()
    {
        $this->validation();
        $this->insert();
        $this->email();
    }
 
    private function validation()
    {
        // Form doğrulaması
    }
 
    private function insert()
    {
        // Kaydetme işlemi
    }
 
    private function email()
    {
        // Kullanıcıya e-posta gönderimi
    }
 
}
</code></pre>

Bu tür bir yapı kurduğunuzda tek sorumluluk ilkesini uygulamış oluyorsunuz. Ancak çağırdığınız alt metotlarda oluşacak bir hatada bir sonraki adıma devam etmeden ilgili hatayı kullanıcıya göstermeniz gerekiyor. Bu tür durumlarda genel itibariyle bir çok developer (eskiden ben de buna dahildim) bir metotun işini sorunsuz yapıp yapmadığını anlamak için true-false gibi bir değer döndürmesini bekliyor. Bu yöntemi kullandığınızda ciddi bir kod karmaşası ortaya çıkıyor. Tıpkı aşağıdaki gibi;

<pre><code class="language-php">
class UserInsertController
{
 
    public function provider()
    {
        $result = $this->validation();        
        if (!$result) return false;
        
        $result = $this->insert();        
        if (!$result) return false;
        
        $result = $this->email();        
        if (!$result) return false;
        
    }
 
    private function validation()
    {
        // Form doğrulaması
        return true;
    }
 
    private function insert()
    {
        // Kaydetme işlemi
        return true;
    }
 
    private function email()
    {
        // Kullanıcıya e-posta gönderimi
        return true;
    }
 
}
</code></pre>

Bu kodun zararsız olduğunu düşünebilirsiniz. Ancak bu yapı çok tehlikelidir ve çok adım gerektiren ve birden fazla durumu barındıran işlerde ortalık karışabilir. Örneğin validasyon işleminde ne tür bir hata var bilmiyoruz. Bunu öğrenmek için ayrıca hatayı da kontrol etmemiz gerekir. Lakin try-catch yapısı bu durumlar için idealdir;

<pre><code class="language-php">
class UserInsertController
{
 
    public function provider()
    {
        try {
            $this->validation();
            $this->insert();
            $this->email();            
        } catch (Exception $e) {
            echo $e->getMessage();
        }
    }
 
    private function validation()
    {
        // Form doğrulaması
        if ($email === '') {
            throw new Exception('E-posta adresi zorunlu.');
        }
    }
 
    private function insert()
    {
        // Kaydetme işlemi
    }
 
    private function email()
    {
        // Kullanıcıya e-posta gönderimi
    }
 
}
</code></pre>

Bu yapıya baktığınızda tek sorumluluk ilkesini hem uyguladığımızı hem de kod karmaşasını engellediğimizi görüyoruz. Ayrıca hata durumunda diğer işlemler de otomatik olarak duruyor. Hata mesajını da sorunsuz bir şekilde alabiliyoruz. provider metodumuz içerisinde bir üst tarafa da hatayı bildirebilirdik. Böylece her bölüm tamamen birbirinden bağımsız olarak çalışmış olurdu. Bu yapıda hiç bir metot başka bir yerde oluşan hayatı kontrol etmek zorunda kalmaz.
