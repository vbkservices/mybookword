# 資料庫
* 主鍵 id
* table ;
* char()文字 
* int()數字
* datetime()(生日,特殊表格)
* varchar(text)(內文,多輸出文字)
---
* like是 % 關鍵 (軟限制)
* = 是完全符合 (硬限制)
* AS 設定欄位標題
* total 欄位簡化為total
* order by 排序 ;
* group by 欄位名 ;把相同的結果結合在一起
 ---
* 參數
* sum 欄位全部資料加總再一起, 加總
* AVG() –返回組的平均值。
* COUNT() -計算資料數量
* MAX() –返回組中的最大值
* MIN() –返回組中的最小值
---
* 數字 +-*/ % ()
* 邏輯 > < <= >= <> =  and or not
* 字串 || 連接字串
* NULL IS NULL, IS NOT NULL


---
### 排序 Order by
select * from 表格名 order by 
#### DESC 由多到少 大到小
select * from 表格名 order by 欄位8 DESC 
#### ASC 由少到多 小到大
select * from 表格名 order by 欄位8 ASC

---
### DISTINCT 唯一 重複的資料只顯示一次
select DISTINCT 欄位6 FROM from 表格名

---
### GROUP BY 把相同的結果結合在一起
select 欄位6, sum(欄位8) from 表格名 group by 欄位6 

---
### Having 配合group by 限定查出來的資料
select 欄位6,sum(欄位8) from 表格名 group by 欄位6 having sum (欄位8) > 10 

---
### Limit用來限制查詢回傳的資料筆數
select * from main order by birthday DESC     limit 3找前3年輕

---
### Offset:用來配合limit往後找找
limit 2 offset 3 找第4-5筆
select * from main order by birthday  DESC     limit 2(顯示的筆數) offset 3(從第幾開始)

### 修改 update
修改 update 表格名 set 修改的欄位名 = ? where 指定的欄位 = ? ;

---
### 查詢 select
查詢 select *(任意欄位) from 表格名 ;
查詢 select * from 表格名 where 欄位名(條件)(例如:年齡小於30(ago < 30)) like '% 關鍵字 %';

---
### 新增 inesrt into
inesrt into 表格名 values (欄位名,欄位名,欄位名,.....) values('修改名',修改名,'修改名',.....);

---
### 刪除 delete
delete * from 表格名 where 欄位名(條件)(例如:年齡小於30(ago < 30)) like '% 關鍵字 %';

---
### 練習題題目
```
請查出住址在高雄的銷售點（show出SQL語法）
select * from  service where addr like '高雄%' ;
 
請查出7xx系列車種的價格（show出SQL語法）
select model,price from  car where model like '7%' ;

請算出 BMW時尚生活館 2004年的銷售總金額
elect sum(sale.num*car.price) from sale,service,car where sale.sid=service.sid and sale.cid=car.cid and service.name like 'BMW%' 
 
請計算BMW公司2004年一、二、三月銷售金額各為多少？（show出SQL語法）
select sale.mon,sale.num*car.price  as total from sale,car where car.cid=sale.cid group by mon order by total; 

請計算3xx 5xx 6xx 7xx 四個系列的車型總銷售金額為多少？（show出SQL語法）
select sum(car.price*sale.num),car.model from sale,car where car.cid=sale.cid and car.model like '5%';

每銷售一台車，銷售點的利潤是10%，請問北、中、南區各獲得多少利潤？（show出SQL語法）
select sum(car.price*sale.num*0.1),service.area from sale,service,car where sale.sid=service.sid and sale.cid=car.cid group by service.area ;
```

### 聖經
第一題
```
select sum(tal) from(select engs,MAx(chap)as tal from nstrkjv group by engs);
共 1189 章
select count(*) from nstrkjv ;
共 31102 節
```
第一題
```
select count(*),main.chinesef from main,nstrkjv where main.engs=nstrkjv.engs group by chinesef order by count(*) DESC ;
答案:詩篇 共2461節
```
