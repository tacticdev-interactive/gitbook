---
description: "\U0001F914 จะทำงานได้ต้องรู้เรื่องฐานข้อมูลแค่ไหนกันนะ ?"
---

# 👶 บทสรุปฐานข้อมูล

ในรอบนี้ **ดช.แมวน้ำ** จะมาสรุป **`ความรู้ขั้นต่ำสุด`** ในการทำงานกับฐานข้อมูล **`ในระดับที่เอาไปทำงานจริงได้`** ให้เพื่อนๆได้เข้าใจกันว่า การทำงานและการออกแบบฐานข้อมูล เขามีวิธีคิด วิธีออกแบบยังไงบ้าง ซึ่งคนไม่มีความรู้เรื่องนี้เลยก็สามารถเข้าใจได้ง่ายๆแน่นอนฮ๊าฟ

{% hint style="danger" %}
**คำเตือน**  
ความรู้ที่จะสอนต่อไปนี้น่าจะเรียกได้ว่า **ถ้าไม่รู้เรื่องพวกนี้ ก็น่าจะไปสายอาชีพนี้ได้ยากม๊วก** ดังนั้นลองเช็คคร่าวๆหน่อยว่ามีข้อไหนที่ตัวเองยังตกหรือเปล่า
{% endhint %}

{% hint style="success" %}
**เกร็ดความรู้**  
Database ในโลกปัจจุบันนั้นมีทั้งหมด 4 ตระกูลหลัก **Graph**, **Key-Value store**, **Document database**, **Column store** ซึ่งแต่ละตระกูลนั้นมันเก่งคนละด้านกัน ซึ่งในรอบนี้เราจะมาเรียนการออกแบบโดยใช้ database ที่ทั่วโลกยังคงนิยมใช้กันอยู่นั่นคือ Relational Database นั่นเอง ซึ่งถ้าเราเข้าใจการออกแบบตัวนี้แล้ว ก็น่าจะแก้ปัญหาได้ 90% ของโลกใบนี้ได้แล้วล่ะ ส่วนที่เหลืออีก 10% คือกรณีพิเศษที่เราจำเป็นต้องไปใช้ตระกูลอื่นๆนั่นเอง ดังนั้นเรียนตัวนี้ไปก่อนก็ไม่เสียหาย เพราะมันยังเป็นพื้นฐานในการคิดของตระกูลอื่นๆด้วยนั่นเอง
{% endhint %}

## 🗃️ ฐานข้อมูลคือไย ?

เวลาที่เราทำแอพอะไรก็ตามแต่ ถ้าแอพของเรามีการบันทึกข้อมูลของผู้ใช้เอาไว้ เช่น เก็บชื่อผู้ใช้ เก็บที่อยู่ บลาๆ นั่นหมายความว่าเราต้องเอาข้อมูลพวกนั้นไปเซฟเก็บไว้ซักที่ ชิมิ? โดยที่ๆเราเอาข้อมูลพวกนั้นไปเซฟไว้นั่นแหละคือ ฐานข้อมูลรูปแบบหนึ่งนั่นเอง เช่น บันทึกเป็นไฟล์เก็บไว้ หรือเชื่อมต่อไปยัง Database ชนิดต่างๆยังไงก็ตาม สิ่งที่แรกเราควรรู้ก่อนทำงานกับฐานข้อมูลก็คือ `💖 วิธีการออกแบบฐานข้อมูล` ซึ่งขั้นตอนในการออกแบบ **ดช.แมวน้ำ** ได้แยกออกเป็นเรื่องๆด้านล่างไว้เรียบร้อยแล้วครัช

## 1.มโน

การออกแบบ Database เริ่มต้นที่การ **`ลองมโนสิ่งที่เก็บในระบบออกมา`**แล้วลองเขียนลงกระดาษเล่นดู ซึ่งอยากฟุ้งซ่านแค่ไหนก็จัดเต็มได้เลย ตามในวีดีโอด้านล่าง

{% embed url="https://youtu.be/9NNKGI\_VdVQ" %}

## 2.จัดกลุ่มกำจัดขยะ

หลังจากที่ฟุ้งซ่านเสร็จเราก็**`เอาทั้งหมดมาลองจัดกลุ่มดู`** จัดผิดจัดถูกก็ทำๆไปเถอะ และหลังจากที่จัดกลุ่มเสร็จ เราก็จะมาดูว่าข้อมูลที่อยู่ในแต่ละกลุ่มนั้น **`มันจำเป็นต่อระบบหรือเปล่า?`** ซึ่งอันไหนที่เราคิดว่ามันไม่จำเป็นเราก็ไล่ลบของพวกนั้นทิ้งไปซะ 

{% embed url="https://youtu.be/4-8Jy7oZ8JY" %}

> ตัดๆทิ้งไปก่อน ถ้ามันจะเป็นเดี๋ยวมันก็กลับมาเอง ส่วนที่ลิสต์ไว้จะถูกกลุ่มหรือเปล่าก็ช่างมัน เดี๋ยวทำไปเรื่อยๆก็จะรู้เองเช่นกัน

## 3.จำลองข้อมูลเล่นดู และ กำหนดคีย์หลัก \(Primary key\)

หลังจากที่จัดกลุ่มเสร็จหมดละ ถัดไปเราก็จะ **`จำลองข้อมูล`** กันเล่นๆ เพื่อที่จะดูว่าสิ่งที่เราคิดไว้มันดูแล้วโอเคหรือเปล่า ซึ่งถ้าดูแล้วมันพอที่จะใช้ได้อยู่ ถัดไปเราก็จะต้องทำการสร้าง **`Primary Key`** ให้มัน เพื่อให้เราสามารถสั่งดำเนินการต่างๆได้ถูก record ที่เราอยากจะทำ

{% embed url="https://youtu.be/ipSSwIaXZHA" %}

{% hint style="info" %}
**Primary Key**  
คือ **คีย์หลัก** ที่เป็นตัวแทนอ้างถึงข้อมูล record นั้นๆ ซึ่งมันจะต้องมีคุณสมบัติคือ Unique หรือพูดง่ายๆคือ มันห้ามซ้ำกับใครเลยนั่นเอง
{% endhint %}

{% hint style="info" %}
**Candidate Key**  
คือ **คีย์ตัวเลือก** มันคือข้อมูลทีมีคุณสมบัติเหมือน Primary Key ทุกประการ เพียงแค่เราไม่ได้เลือกมันเป็น Primary Key เฉยๆ เช่น เวลาเราเป็นนักเรียน เราก็จะมีรหัสประจำตัวนักเรียนชิมิ? ซึ่งรหัสประจำตัวนักเรียนแต่ละคนก็ไม่ซ้ำกันเลย และ เราใช้รหัสประจำตัวนักเรียนเป็น Primary Key เพราะเวลาคุณครูจะพูดถึงเรา เขาก็จะพูดรหัสประจำตัวนักเรียนยังไงล่ะ แต่จริงๆแล้วเราก็มี รหัสประชาชน อยู่เหมือนกัน ซึ่งมันก็ไม่ซ้ำกับใครเลยในโรงเรียนของเราเช่นกัน แต่ในโรงเรียนไม่นิยมเรียกนักเรียนด้วยรหัสประชาชน ดังนั้นในกรณีนี้ รหัสประชาชนเลยเป็นแค่ Candidate Key เท่านั้น
{% endhint %}

## 4.ความสัมพันธ์ของตาราง

คราวนี้เราก็จะมาดูความสัมพันธ์ของตารางแต่ละตัวกัน ซึ่งหลักๆจะมีทั้งหมด 3 รูปแบบคือ `1:1` , `1:Many` และ `Many:Many` โดยที่ความสัมพันธ์แต่ละรูปแบบนั้นก็จะมีวิธีในการออกแบบที่แตกต่างกันไป ตามในวีดีโอด้านล่าง

{% embed url="https://youtu.be/ivFAp\_LNfMU" %}

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xE04;&#xE27;&#xE32;&#xE21;&#xE2A;&#xE31;&#xE21;&#xE1E;&#xE31;&#xE19;&#xE18;&#xE4C;</th>
      <th
      style="text-align:left">&#xE01;&#xE32;&#xE23;&#xE2D;&#xE2D;&#xE01;&#xE41;&#xE1A;&#xE1A;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">1:1</td>
      <td style="text-align:left">&#xE40;&#xE25;&#xE37;&#xE2D;&#xE01; Primary Key &#xE02;&#xE2D;&#xE07;&#xE15;&#xE32;&#xE23;&#xE32;&#xE07;&#xE44;&#xE2B;&#xE19;&#xE01;&#xE47;&#xE44;&#xE14;&#xE49;
        &#xE44;&#xE1B;&#xE43;&#xE2A;&#xE48;&#xE2D;&#xE35;&#xE01;&#xE15;&#xE32;&#xE23;&#xE32;&#xE07;&#xE2B;&#xE19;&#xE36;&#xE48;&#xE07;
        <br
        />&#xE40;&#xE1E;&#xE37;&#xE48;&#xE2D;&#xE43;&#xE0A;&#xE49;&#xE40;&#xE1B;&#xE47;&#xE19;
        Foreign Key</td>
    </tr>
    <tr>
      <td style="text-align:left">1:Many</td>
      <td style="text-align:left">&#xE40;&#xE25;&#xE37;&#xE2D;&#xE01; Primary Key &#xE02;&#xE2D;&#xE07;&#xE15;&#xE32;&#xE23;&#xE32;&#xE07;&#xE17;&#xE35;&#xE48;&#xE40;&#xE1B;&#xE47;&#xE19;
        1 &#xE44;&#xE1B;&#xE43;&#xE2A;&#xE48;&#xE43;&#xE19;&#xE15;&#xE32;&#xE23;&#xE32;&#xE07;&#xE17;&#xE35;&#xE48;&#xE40;&#xE1B;&#xE47;&#xE19;
        Many
        <br />&#xE40;&#xE1E;&#xE37;&#xE48;&#xE2D;&#xE19;&#xE33;&#xE44;&#xE1B;&#xE40;&#xE1B;&#xE47;&#xE19;
        Foreign Key</td>
    </tr>
    <tr>
      <td style="text-align:left">Many:Many</td>
      <td style="text-align:left">
        <p>&#xE2A;&#xE23;&#xE49;&#xE32;&#xE07;&#xE15;&#xE32;&#xE23;&#xE32;&#xE07;&#xE43;&#xE2B;&#xE21;&#xE48;&#xE02;&#xE36;&#xE49;&#xE19;&#xE21;&#xE32;
          &#xE42;&#xE14;&#xE22;&#xE19;&#xE33; Primary Key &#xE02;&#xE2D;&#xE07;&#xE17;&#xE31;&#xE49;&#xE07;
          2 &#xE15;&#xE32;&#xE23;&#xE32;&#xE07;&#xE40;&#xE02;&#xE49;&#xE32;&#xE44;&#xE1B;&#xE43;&#xE19;&#xE15;&#xE32;&#xE23;&#xE32;&#xE07;&#xE43;&#xE2B;&#xE21;&#xE48;</p>
        <p>&#xE40;&#xE1E;&#xE37;&#xE48;&#xE2D;&#xE19;&#xE33;&#xE44;&#xE1B;&#xE40;&#xE1B;&#xE47;&#xE19;
          Foreign Key &#xE41;&#xE25;&#xE30;&#xE41;&#xE19;&#xE30;&#xE19;&#xE33;&#xE43;&#xE2B;&#xE49;&#xE2A;&#xE23;&#xE49;&#xE32;&#xE07;
          Primary Key &#xE02;&#xE2D;&#xE07;&#xE15;&#xE31;&#xE27;&#xE40;&#xE2D;&#xE07;&#xE02;&#xE36;&#xE49;&#xE19;&#xE21;&#xE32;&#xE14;&#xE49;&#xE27;&#xE22;</p>
      </td>
    </tr>
  </tbody>
</table>## **5.Normalization**

จากขั้นตอนทั้งหมดที่ทำไปด้านบน เราจะถือว่าเป็นการออกแบบฐานข้อมูลแบบกากๆ ยังไม่เหมาะสมที่จะเอาไปใช้งานจริง เพราะมันจะมีหลายอย่างที่ทำให้เราทำงานกับฐานข้อมูลได้ยาก เช่น ของบางอย่างไม่ควรอยู่ในตารางนั้น หรือ มันมีข้อมูลซ้ำกันอยู่เต็มไปหมด ดังนั้นในรอบนี้เราจะมาใช้หลัก **`Normalization`** เพื่อที่จะลดปัญหาที่ว่ามาพวกนั้นดูละกัน

### เกริ่น

{% embed url="https://youtu.be/Zq6Od1d2Z1w" %}

### 1st Normal Form

{% embed url="https://youtu.be/dyKeNkSvp5s" %}

### 2nd & 3rd Normal Forms

{% embed url="https://youtu.be/3gda4S-V7ck" %}

{% hint style="warning" %}
**บทความนี้กำลังทำอยู่** คาดว่าจะเสร็จราวๆวันอาทิตย์ที่ 23/02/2020 นี้แหละ ส่วนใครที่ไม่อยากพลาดอัพเดทก็เข้าไปกดติดตามที่ลิงค์นี้ [**Mr.Saladpuk**](https://www.facebook.com/mr.saladpuk) ได้เลย และถ้าช่วยกดแชร์ด้วยจะเป็นพระคุณอย่างมากครัช
{% endhint %}

## วีดีโอตัวถัดไปที่จะทำ

* Tips การใช้งานจริง
* * Denormalization
  * Bottlenecks
  * Paging
  * Calculation value
  * Lean computations
* ~~Design Document database \(MongoDB\)~~ **เดี๋ยวเปิดสอนการทำ MongoDb เป็นอีกคอร์สเลยละกัน**
* คิดไรได้เดี๋ยวเอามาใส่ต่อ

{% page-ref page="img-handling.md" %}

{% page-ref page="database-indexing.md" %}

{% page-ref page="delete-records.md" %}

