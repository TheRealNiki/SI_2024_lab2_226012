Nikola Golubovski 226012



2. ![Control Flow diagram drawio](https://github.com/TheRealNiki/SI_2024_lab2_226012/assets/138707527/c56d184e-adc6-4a1a-82ac-8b6efa913127)


3. Цикломатската комплексност е метрика која ја мери комплексноста на програмата преку броење на независните патеки низ графот на контролниот тек на програмата (Control Flow Graph, CFG). Цикломатската комплексност може да се пресмета на два различни начини и тоа: број на ребра - број на јазли + 2; број на преикатни јазли + 1. Јас добив 25 јазли, 33 ребра па 33-25+2 = 10 и 9 предикатни јазли па 9+1 = 10, од каде што следи дека цикломатската комплесност е 10.
4. Критериумот Every Branch (секоја гранка) бара да се креираат тест случаи кои ќе осигураат дека секоја гранка од секој услов во кодот е извршена барем еднаш.



    @Test
    void testAllItemsNull() {
        // Тест случај 1: allItems == null
        RuntimeException exception = assertThrows(RuntimeException.class, () -> {
            SILab2.checkCart(null, 100);
        });
        assertEquals("allItems list can't be null!", exception.getMessage());
    }

    @Test
    void testItemNameNull() {
        // Тест случај 2: item.getName() == null
        allItems.add(new Item(null, "123456", 100, 0));
        assertTrue(SILab2.checkCart(allItems, 100));
        assertEquals("unknown", allItems.get(0).getName());
    }

    @Test
    void testItemNameEmpty() {
        // Тест случај 3: item.getName().length() == 0
        allItems.add(new Item("", "123456", 100, 0));
        assertTrue(SILab2.checkCart(allItems, 100));
        assertEquals("unknown", allItems.get(0).getName());
    }

    @Test
    void testValidBarcode() {
        // Тест случај 4: item.getBarcode() != null
        allItems.add(new Item("item1", "123456", 100, 0));
        assertTrue(SILab2.checkCart(allItems, 100));
    }

    @Test
    void testBarcodeNull() {
        // Тест случај 5: item.getBarcode() == null
        allItems.add(new Item("item1", null, 100, 0));
        RuntimeException exception = assertThrows(RuntimeException.class, () -> {
            SILab2.checkCart(allItems, 100);
        });
        assertEquals("No barcode!", exception.getMessage());
    }

    @Test
    void testInvalidCharacterInBarcode() {
        // Тест случај 6: Invalid character in barcode
        allItems.add(new Item("item1", "123a56", 100, 0));
        RuntimeException exception = assertThrows(RuntimeException.class, () -> {
            SILab2.checkCart(allItems, 100);
        });
        assertEquals("Invalid character in item barcode!", exception.getMessage());
    }

    @Test
    void testDiscountGreaterThanZero() {
        // Тест случај 7: item.getDiscount() > 0
        allItems.add(new Item("item1", "123456", 100, 0.1f));
        assertTrue(SILab2.checkCart(allItems, 100));
    }

    @Test
    void testDiscountEqualToZero() {
        // Тест случај 8: item.getDiscount() == 0
        allItems.add(new Item("item1", "123456", 100, 0));
        assertTrue(SILab2.checkCart(allItems, 100));
    }

    @Test
    void testPriceGreaterThan300WithDiscountAndBarcodeStartsWith0() {
        // Тест случај 9: item.getPrice() > 300 && item.getDiscount() > 0 && item.getBarcode().charAt(0) == '0'
        allItems.add(new Item("item1", "012345", 400, 0.1f));
        assertFalse(SILab2.checkCart(allItems, 100));
    }

    @Test
    void testSumLessThanOrEqualToPayment() {
        // Тест случај 10: sum <= payment
        allItems.add(new Item("item1", "123456", 100, 0));
        allItems.add(new Item("item2", "234567", 200, 0));
        assertTrue(SILab2.checkCart(allItems, 300));
    }

    @Test
    void testSumGreaterThanPayment() {
        // Тест случај 11: sum > payment
        allItems.add(new Item("item1", "123456", 100, 0));
        allItems.add(new Item("item2", "234567", 200, 0));
        assertFalse(SILab2.checkCart(allItems, 250));
    }



Објаснување на тест случаите
testAllItemsNull: Проверува дали функцијата фрла исклучок кога allItems е null.
testItemNameNull: Проверува дали името на предметот се поставува на "unknown" кога името е null и пресметувајќи ја сумата.
testItemNameEmpty: Проверува дали името на предметот се поставува на "unknown" кога името е празно и пресметувајќи ја сумата.
testValidBarcode: Проверува нормална обработка на предмет со валиден баркод.
testBarcodeNull: Проверува дали функцијата фрла исклучок кога баркодот е null.
testInvalidCharacterInBarcode: Проверува дали функцијата фрла исклучок кога баркодот содржи невалиден карактер.
testDiscountGreaterThanZero: Проверува пресметка на сумата со попуст.
testDiscountEqualToZero: Проверува пресметка на сумата без попуст.
testPriceGreaterThan300WithDiscountAndBarcodeStartsWith0: Проверува одземање на 30 од сумата кога се исполнети условите.
testSumLessThanOrEqualToPayment: Проверува враќање true кога сумата е помала или еднаква на уплатата.
testSumGreaterThanPayment: Проверува враќање false кога сумата е поголема од уплатата.



5. 

    @Test
    void conditionTest() {
        // T T T
        RuntimeException ex;
        ex = assertThrows(RuntimeException.class, () -> SILab2.checkCart(List.of(new Item("ime", "025", 500, 0.1f)), 100));
        assertTrue(ex.getMessage().equals("Condition reached!"));

        // T T F
        boolean result3 = SILab2.checkCart(List.of(new Item("ime", "225", 500, 0.1f)), 100);
        assertTrue(result3);

        // T F *
        boolean result4 = SILab2.checkCart(List.of(new Item("something", "123", 400, 0.0f)), 100);
        assertFalse(result4);

        // F * *
        boolean result5 = SILab2.checkCart(List.of(new Item("something", "123", 200, 0.0f)), 100);
        assertFalse(result5);
    }
}

T T T: Овој тест случај проверува дали сите услови се вистинити. Кога сите услови се исполнети, кодот треба да влезе во if условот и да фрли RuntimeException со порака "Condition reached!". Ова се потврдува преку проверка на пораката од исклучокот.

T T F: Овој тест случај проверува комбинација каде што првите два услови се вистинити, а третиот е невистинит. Кодот не треба да влезе во if условот и треба да врати true, што е и добиениот резултат.

**T F ***: Овој тест случај проверува комбинација каде што првиот услов е вистинит, а вториот е невистинит. Без разлика на вредноста на третиот услов, кодот не треба да влезе во if условот и треба да врати false.

**F ** ***: Овој тест случај проверува комбинација каде што првиот услов е невистинит. Без разлика на вредноста на останатите услови, кодот не треба да влезе во if условот и треба да врати false.

Со овие тест случаи, се покриваат сите можни комбинации на услови и се обезбедува целосно тестирање според Multiple Condition критериумот.

