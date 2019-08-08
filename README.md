# Автозаказ_Репозиторий
База рабочих версий Автозаказа


**00**

--Drop table NewGen.PLA_MARA;
--Drop table NewGen.PLA_MARM;

Drop table koshovec.PLA_MARA;
Create table koshovec.PLA_MARA
as
select
   ltrim("Материал", '0')   "Материал",
   "Мдт",
   "Вид материала",
   "Группа материалов",
   "БЕИ",
   "ЕИ заказа на поставку",
   "Вес брутто",
   "Вес нетто",
   "ЕИ веса",
   "Объемы",
   "ЕИ объема",
   "Уровень планирования потребн",
   "Европейский номер товара (EAN)",
   "Длина",
   "Ширина",
   "Высота",
   "ЕИ размеров",
   "Коэффициент штабелирования",
   "Остаточный срок годности",
   "Общий срок годности",
   "Процентная ставка хранения",
   "МинСГ/дата истеч срока хранен",
   "Код периода для МСГ",
   "Полное наименование материала",
   "Статус ассортимента для мат",
   "Тип уровня цен",
   "Дата создания",
   "Дата последнего изменения"
from
   NewGen.ZDB_MARA
;
alter table PLA_MARA add Unique ("Материал");


Drop table NewGen.TB_ZASSTAT_HIST;
Create table NewGen.TB_ZASSTAT_HIST
as
select 
   ltrim("Материал", '0')   "Материал",
   "Порядковый номер",
   "Статус ассортимента для матер",
   "Дата последнего изменения",
   to_date('1970-01-01 ' || "Время изменения (текст)", 'YYYY-MM-DD HH24:MI:SS')   "Время изменения",
   "Время изменения (текст)",
   "Дата начала действия статуса"
from 
   NEWGEN.ZDB_ZASSTAT_HIST
;
alter table NEWGEN.TB_ZASSTAT_HIST add Unique ("Материал", "Порядковый номер");


Drop table koshovec.PLA_ZASSTAT;
Create table koshovec.PLA_ZASSTAT
as
select
   T1.ZZSTATUSASS   "Статус ассортимента для матер",
   T2.TEXT   "Название статуса ассортимента",
   T1.MMSTA   "Статус закупки", 
   T3.MTSTB   "Название статуса закупки", 
   T1.MSTAV   "Статус продажи",
   T4.VMSTB   "Название статуса продажи",
   T2.SPRAS   "Язык"
from
   Replic.ERP_ZASSTAT   T1
   inner join
   Replic.ERP_ZASSTATT   T2
      on T2.MANDT = T1.MANDT
      and T2.ZZSTATUSASS = T1.ZZSTATUSASS
   inner join
   Replic.ERP_T141T   T3
      on T3.MANDT = T1.MANDT
      and T3.SPRAS = T2.SPRAS
      and T3.MMSTA = T1.MMSTA
   inner join
   Replic.ERP_TVMST   T4
      on T4.MANDT = T1.MANDT
      and T4.SPRAS = T2.SPRAS
      and T4.VMSTA = T1.MSTAV
Where
   T1.MANDT = 400
   and
   T2.SPRAS = 'R'
;
alter table koshovec.PLA_ZASSTAT add Unique ("Статус ассортимента для матер", "Язык");


drop table koshovec.PLA_MARM;
create table koshovec.PLA_MARM
as
select
ltrim("Материал", '0')   "Материал",
"Альтернативная ЕИ",
"Числитель для пересчет в БЕИ",
"Знаменатель для пересчет в БЕИ",
"Код EAN",
"Тип EAN",
"Объемы",
"ЕИ объема",
"Вес брутто",
"ЕИ веса",
"Длина",
"Ширина",
"Высота",
"ЕИ размеров"
from
   NewGen.ZDB_MARM;
alter table koshovec.PLA_MARM add Unique ("Материал", "Альтернативная ЕИ");


drop table koshovec.pla_t006;
create table koshovec.pla_t006
as
select
T1."Внутренняя ЕИ",
T1."3-значная внеш. ЕИ",
T1."6-значная внеш. ЕИ",
T1."ДесРазрДляОкругления",
T1."Коммерческая ЕИ",
T1."Стоимостное облиго",
T1."(1)-единица",
T1."(2)-единица",
T1."Величина",
T1."Счетчик",
T1."Знаменатель",
T1."Показатель",
T1."ДополнитКонстанта",
T1."ЭкспонентПлавЗапятой",
T1."Десятичные разряды",
T1."Код ISO",
T1."Начальный код",
T1."ЗнТемпературы",
T1."ЕИ температуры",
T1."Семейство ЕИ",
T1."Значение печати",
T1."Единица печати",
T2."Код языка",
T2."Коммерч.",
T2."Техническое",
T2."Текст_1 к ЕИ",
T2."Текст_2 к ЕИ"
from
   replic.erp_t006   T1
   left outer join
   replic.erp_t006a   T2
      on T2."Мандант" = T1."Мандант"
      and T2."Внутренняя ЕИ" = T1."Внутренняя ЕИ"
      and T2."Код языка" = 'R'
where
   T1."Мандант" = 400
;


drop table koshovec.pla_EINA;
create table koshovec.pla_EINA
as
select
"Инфо-запись",
"Дата создания записи",
"Лицо, создавшее запись",
"Материал",
"Поставщик",
"Метка главного поставщика",
"ЕИ заказа на поставку"   "Название ЕИЗ"
from
   NewGen.tmp_EINA
where
"Пометить для удаления" is null
;
alter table koshovec.pla_EINA add Unique ("Инфо-запись");
alter table koshovec.pla_EINA add ("ЕИЗ" varchar2(100));

--select * from koshovec.pla_EINA;


Drop table IK05_pla_ZMAT_RESD;
Create table IK05_pla_ZMAT_RESD
as
--Select * From NEWGEN.Z14_KNA1_OLD_4
Select * From NEWGEN.ZDB_ZMAT_RESD
;

exit;

--select max("Дата") from NEWGEN.ZDB_ZMAT_RESD;
--select * from IK05_pla_ZMAT_RESD where "Дата" >= date '2018-11-02';
