# What is Virtual IXP

在了解什麼是虛擬IXP以前，先了解什麼是「網」吧!  
這裡的網，是指大家平常上網的網   
我一開始是因為興趣，接入了 DN42 網，後來取得真實 ASN  
我們把真正幫我們傳遞封包的稱作「專業ISP」，我這種業餘的叫做「Player」吧  
講的可能有誤，歡迎糾錯

## 專業ISP
真正的 ISP 的網，大概像這樣子  
![](https://i.imgur.com/EUixT9N.png)  
有著自己的的眾多節點，提供著各式各樣的服務  
個個節點和資料中心之間，使用物理電路/光纜線相連  
並且給客戶提供服務時，也是真的拉一條線去那邊(比如物理上拉了一條線到你家，讓你上網)  

## 業餘 Player
至於業餘的網，大概長這樣子  
有許多節點，但幾乎都是VM，或是租用別人的基礎設施  
VM之間也沒有自己的基礎光纖設施，而是**使用隧道來「模擬」光纜**，使節點間互聯  
![](https://i.imgur.com/ukRolkU.png)  

## 網路互連
每家業者都有自己的眾多節點，節點之間互相拉光纖來傳輸流量  
但是，不同業者之間，也需要交換流量。這就是「互連」  
中華電信有自己的網，遠傳電信也有自己的網。  
但是在中華電信和遠傳之間，各有一個(可能有多個)節點是**戶相連起來**的這就是「互聯」  
每當中華電信的使用者，想要連線遠傳的網站時，就要經過這這個互連點   
![](https://i.imgur.com/xC9vlU5.png)  
互連點之間，也是透過光纖連接  
因為這條光纖同時連接兩家業者，所以可能會拆帳，比如建設費用你付一半我付一半之類  

## IXP: 網路交換中心
但是這個時候，就會遇到問題了  
現在業者越來越多了，但是每家業者之間都要兩兩互連  
就會像這樣子，要蓋的光纜線越來越多了，呈現指數上漲  
![](https://i.imgur.com/TYyAiNt.png)  
於是，就有了「網路交換中心(IXP)」的概念出現了  
只要像這樣，大家都連線到交換中心，就可以和其他 ISP 互連了  
![](https://i.imgur.com/iH6Nm5w.png)  

這樣子的設計，簡化了網路的互聯難度，養增加了網路的互連度  

如果 IX 的成員增多了，互連度就會增加  
網路間封包流動的成本降低，延遲也降低了，**同時也增加了網路的吞吐量**  

## 物理 IXP
而物理 IXP 就是**真的有光纜**  

每家業者都有著眾多的節點，甚至有著自己的資料中心，提供各式各樣的服務  
但是現在業者間不用互相喬攏，誰蓋光纖線誰出錢  
只要大家一起挖馬路，從自己的節點蓋光纖到交換中心就好了  
大家一起把網路線插到 IXP 的交換機上面，然後交換路由和流量(也就是跳線接入)  
或是 IXP colo ，把自己的路由器直接放在 IXP 的機櫃裡面  
然後搭配「進線服務」，把自己的外部連點和自己放在 IXP 機櫃的路由器，物理上面連接起來  
![](https://i.imgur.com/0HXWiAX.png)  

「進線服務」是指 colo 了以後，雖然在 IXP 內有了節點。但是**光有 IXP 內孤立的節點也沒用**  
還是**要有光纜線連起自己和其他節點之間**才行，這樣才能將自己的業務流量送入 IXP 裡面交換  
進線服務，就提供了解決方案。他能幫忙你連接起 IXP 內的節點和自己的外部節點  

進線服務和 IXP 自身幾乎同等重要。[這篇文章](https://www.ptt.cc/bbs/Broad_Band/M.1239365152.A.886.html) 就吐槽了中華電信一直死守 TWIX 不讓其它人進線  
連進 IXP LAN 的成本比直接蓋互連光纜還要高出 N 倍，也為台灣的畸形的網路環境推了一把(最近稍微好了一點，但還是遠遠比不上香港/新加坡/日本)  
如果有一個 IX 不管是把封包送進去，還是把封包送出來都難如登天。那也沒什麼人願意在這邊交換  

順帶一提「port接入、colo、跳線、進線」 分別是獨立不相干服務，甚至可能由不同提供商提供  

以 STUIX 為例  
STUIX 提供了「port 接入」、「colo」服務  
是方電訊提供了「跳線」、「進線」和「colo」服務  
要互相搭配使用  

## 虛擬 IXP

虛擬 IXP 則是類似這張圖，「模擬」物理 IXP，讓業餘愛好者體驗接入 IXP 的感覺  
![](https://i.imgur.com/jt1pPjI.png)  

1. 隧道接入: 模擬直接由外部跳線接入。從自己外部的節點，直接連接到 IX 的交換機上
2. VM 接入: 
    1. VM本體: 模擬把自己的機器部屬在 IXP 裡面
    2. 乘載網路: 模擬外部進線服務，讓大家打隧道去自己其他的內網節點。通常被稱作「上網卡(eth0)」

和物理IX不同，虛擬IX 做為教育和實驗用途。他**不會改善網路的吞吐量**  
以 Poema IX 為例，因為(除了Wifi接入的成員)，物理上仍依托於中華電信的電路  
物理層面的網路拓樸結構沒有改變，因此網路的實際吞吐量也不會變化  

不同於物理 IX 實際業務跑在物理光纖上面，上網卡單純用於管理路由器  
虛擬因為業務都跑在隧道裡面，「上網卡」的網路品質就很重要了  
如果乘載網延遲很高，就會導致這在這邊交換的延遲都被拉高  

## 混合 IXP

還有一種IX: 混合IX。就是類似這種，混和了虛擬IX和實體IX，開放物理接入的同時允許業餘玩家用隧道/VM的方式接入  
![](https://i.imgur.com/l3m9nJk.png)

因為混合了兩種類型的接入成員  
對於實體接入的成員，上網卡能 ssh 就好，延遲就不重要了。物理光纖更重要  
但是對於虛擬接入的成員，因為業務都跑在隧道裡，延遲就變得很重要了。延遲太高會無法使用  

虛擬接入的成員不改變物理拓樸，也不改變整個網路的吞吐量  
因為實際的網路封包類似這樣流動，仍然依託於物理接入成員的線路  
![](https://i.imgur.com/SY5n5Zy.png)
因此混合IX的規模通常更難以估算  

### IX 的上網卡<a name="IX-VM-ETH0"></a>

正式的物理 IX ，需要和「自己的其他內網節點」物理拉光纖線(使用跳線/進線服務)  

比如 Google 和遠傳在 TPIX 交換流量，遠傳的用戶訪問 Gmail  
Google 的 TPIX 節點需要和彰化機房物理上有拉光纖線  
遠傳的 TPIX 節點也和自己機房有物理光纖，再拉光纖到各戶人家  
![](https://i.imgur.com/DAEiOfm.png)

所以**物理 IX 是沒有「上網卡」這個概念的**，都是真的物理線路  
頂多給個 offband 網路，供 ssh 管理使用。但絕對不會在這裡面跑業務流量  

虛擬 IX 和物理 IX 類似，同樣也有「跳線」、「進線」的問題。但這部是**使用隧道來模擬**的  
而**隧道必須跑在乘載網**(IX VM 裡面通常叫做上網卡)上面。因此，乘載網的網路品質就變得很重要了  
如果乘載網品質太差，大家過來的延遲都很高。那麼，在這邊交換的大家延遲都會很糟糕  

以 Poema IX 為例。連出時，乘載網有「中華電信直連」、「WARP VPN(Cloudflare)」、「大易網(感謝小易)」三條線路  
可以選擇適合的線路和自己的內網節點打隧道，降低一點延遲  
但要注意的是，物理上仍依托於中華電信的電路

對虛擬 IX 來說，不管是模擬「跳線」、「進線」，在乘載網的角度上，連線對象都是「自己其他的內網節點」  
Poema IX 提供的 IX VM 乘載網[這條限制](../#IX-VM-LIMIT)就是這樣來的: **禁止流量落地，通訊對象必須是自己其他的內網節點**  
KSKB 提供 VM 的目的很明確: 模擬物理 IX ，供大家測試和練習網路配置。  
而不是免費丟給你一個 VM 隨便架服務，或是落地當成 VPN 出口節點來用  

上乘載網的某些線路(比如大易網)是群友贊助的。所以，使用上也會有一些限制。比如說某些線路要登記 Dst IP 才能用  

## KSKB-IX
KSKB-IX 是「虛擬 IXP」，模擬公網的基礎設施
主要使用[隧道和 VM ](/#join)接入，供業餘愛好和體驗測試用  
示意圖可以看到「VM接入」和「隧道接入」分別模擬的部分  

主機硬體是一台規格不高的電腦 (4 core + 16G RAM)，給所有人都開 kvm 記憶體會不夠  
因此 VM 接入的成員，只能提供 LXC VM(1core/512M 記憶體，合理使用禁止佔滿)   
只能夠真的有需要才開 kvm (比如 RouterOS)  
但不管是 KVM 或是 LXC，都能安裝 bird/frr 開轉發，扮演一個路由器的角色  
![](https://i.imgur.com/PPVWC59.png)

虛擬 IXP 並不提供給商業用途使用，也不能長時間跑大流量占用... 
有此需求請接入物理 IXP 或是商業性質的混合 IXP  
若想測試和學習，或是增加 peer 數量，非常歡迎加入喔  
