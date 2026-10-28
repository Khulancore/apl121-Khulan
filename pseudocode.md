# Мөнгөө өсгөе — псевдокод

Энэ нь `quest.fprg` блок схемийн дарааллыг энгийн үгээр бичсэн хувилбар. Хувьсагчдын нэрийг кодтой нь ижил үлдээсэн.

## Хувьсагчид

| Нэр | Үүрэг |
|---|---|
| `money` | Анхны хуримтлал |
| `add` | Сар бүр нэмэх мөнгө |
| `bank` | Банкны дугаар |
| `rate` | Жилийн хүү |
| `minMoney` | Банкны доод мөнгө |
| `minMonths` | Банкны доод хугацаа |
| `option` | 1 эсвэл 2-р тооцоолол |
| `months` | Хэдэн сар хадгалахыг оруулсан тоо |
| `month` | Хэдэн сар өнгөрснийг тоолно |
| `balance` | Одоогийн үлдэгдэл |
| `goal` | Зорилтот хуримтлал |
| `interest` | Нийт хүү |
| `monthInterest` | Тухайн сарын хүү |

## Блокийн дагуу бичсэн нь

```text
START

    DECLARE money, add, rate, minMoney, balance, goal, interest, monthInterest AS Real
    DECLARE bank, minMonths, option, months, month AS Integer

    // 1. Анхны хуримтлал болон сар бүр нэмэх мөнгө авах
    OUTPUT "Анхны хуримтлалө оруул"
    INPUT money
    OUTPUT "Сар бүр нэмэх мөнгөө оруул"
    INPUT add

    WHILE money < 100000 OR add < 0 DO
        OUTPUT "Мөнгөн дүн буруу. Дахин оруул"
        INPUT money
        INPUT add
    END WHILE

    // 2. Банкны нөхцөл харуулах
    CALL ShowBanks()

    // 3. Банк сонгох
    bank ← 0
    minMoney ← 0
    WHILE bank == 0 DO
        INPUT bank
        IF bank == 1 THEN
            rate ← 0.16
            minMoney ← 1000000
            minMonths ← 6
        ELSE
            IF bank == 2 THEN
                rate ← 0.14
                minMoney ← 100000
                minMonths ← 3
            ELSE
                IF bank == 3 THEN
                    rate ← 0.18
                    minMoney ← 2000000
                    minMonths ← 24
                ELSE
                    OUTPUT "Банкны дугаар буруу"
                    bank ← 0
                END IF
            END IF
        END IF

        IF bank != 0 AND money < minMoney THEN
            OUTPUT "Энэ банкны хугацаа эхлэх мөнгө хүрэхгүй"
            bank ← 0
        END IF
    END WHILE

    // 4. Аль аргаар тооцохыг сонгох
    OUTPUT "1. Хугацаагаар бодох"
    OUTPUT "2. Зорилтот хуримтлалнд хүрэх хугацааг бодох"
    INPUT option
    WHILE option != 1 AND option != 2 DO
        OUTPUT "1 эсвэл 2 гэж оруул"
        INPUT option
    END WHILE

    balance ← money
    month ← 0

    IF option == 1 THEN
        // 5А. Хэдэн сар хадгалахаа оруулах
        INPUT months
        WHILE months < minMonths OR months > 600 DO
            OUTPUT "Хугацаа тохирохгүй. Дахин оруул"
            INPUT months
        END WHILE

        OUTPUT "Сар | Тухайн сарын хүү | Сарын эцсийн үлдэгдэл"
        WHILE month < months DO
            monthInterest ← balance * rate / 12
            balance ← balance + monthInterest + add
            month ← month + 1
            OUTPUT month, monthInterest, balance
        END WHILE

    ELSE
        // 5Б. Зорилтот хуримтлалө оруулах
        INPUT goal
        WHILE goal <= 0 DO
            OUTPUT "Зорилтот хуримтлал 0-ээс их байна"
            INPUT goal
        END WHILE

        OUTPUT "Сар | Тухайн сарын хүү | Сарын эцсийн үлдэгдэл"
        WHILE balance < goal AND month < 600 DO
            monthInterest ← balance * rate / 12
            balance ← balance + monthInterest + add
            month ← month + 1
            OUTPUT month, monthInterest, balance
        END WHILE

        IF balance >= goal THEN
            OUTPUT "Зорилтот хуримтлалндөө хүрлээ"
            IF month < minMonths THEN
                OUTPUT "Банкны доод хугацаа хараахан дуусаагүй"
            END IF
        ELSE
            OUTPUT "600 сард зорилгод хүрсэнгүй"
        END IF
    END IF

    // 6. Үр дүн
    interest ← balance - money - add * month
    OUTPUT "Хугацаа: ", Int(month / 12), " жил ", month % 12, " сар"
    OUTPUT "Нийт оруулсан мөнгө: ", money + add * month
    OUTPUT "Нийт хүү: ", interest
    OUTPUT "Эцсийн хуримтлал: ", balance
END

FUNCTION ShowBanks()
    OUTPUT "Банкны нөхцөл"
    OUTPUT "В: 18%, 2000000₮, 24 сар"
    OUTPUT "А: 16%, 1000000₮, 6 сар"
    OUTPUT "Б: 14%, 100000₮, 3 сар"
END FUNCTION
```

**Блокийн холбоо:** `DECLARE` = Declare, `INPUT` = Input, `OUTPUT` = Output, `←` = Assign, `IF` = If, `WHILE` = While, `CALL` = Call. C кодын мөнгө авах хэсэг `do-while` боловч Flowgorithm дээр `Input`-ийн дараах `While` хэвээр байгаа.
