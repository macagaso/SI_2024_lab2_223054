
# Втора лабораториска вежба по Софтверско инженерство
## Марија Гашоска 223054
### Control Flow Graph
![grafik drawio](https://github.com/macagaso/SI_2024_lab2_223054/assets/139007935/bdcfe31d-6c94-4c50-836c-cb99967b7fad)
### Цикломатска комплексност
Цикломатската комплексност=E(број на ребра)-N(број на јазли)+2 => 36-28+2=10 цикломатската комплексност е 10.
### Тест случаи според Every Branch критериум
Проверки дали функцијата при различни внесови од корисникот се однесува правилно, пример  
allItems е null (RuntimeException со порака "allItems list can't be null!"), 
allItems има елемент со null баркод (RuntimeException со порака "No barcode!"),
allItems има елемент со невалиден баркод, валиден внес за цена, попуст.

```java
@Test
void EveryBranchTest(){
    Item item_invalid_barcode = SILab2.createItem("Chocolate", "12hihi", 170, 0);
    Item item_no_barcode = SILab2.createItem("Juice", null, 170, 0);
    Item item_valid = SILab2.createItem("Strawberries", "872456729", 170, 0);
    Item item_high_price_discount_starting_with_zero = SILab2.createItem("Bread", "012445789", 550, 0.1f);

    RuntimeException ex;
    ex = assertThrows(RuntimeException.class, () -> SILab2.checkCart(null, 0));
    assertTrue(ex.getMessage().contains("allItems list can't be null!"));

    ex = assertThrows(RuntimeException.class, () -> SILab2.checkCart(items(item_invalid_barcode), 0));
    assertTrue(ex.getMessage().contains("Invalid character in item barcode!"));

    ex = assertThrows(RuntimeException.class, () -> SILab2.checkCart(items(item_no_barcode), 0));
    assertTrue(ex.getMessage().contains("No barcode!"));

    assertEquals(true, SILab2.checkCart(items(item_valid), 300));
    assertEquals(false, SILab2.checkCart(items(item_valid), 50));

    assertTrue(SILab2.checkCart(items(item_high_price_discount_starting_with_zero), 1000));
}
```


### Тест случаи според Multiple Condition критериум
Се проверуваат комбинациите FXX,TFX,TTF и TTT, поточно различните комбинации на услови што се проверуваат во функцијата.
Преку тестовите, се креира артикл со соодветни карактеристики и се проверува дали функцијата враќа соодветен резултат во зависност од состојбата на артиклот.
```java
@Test
void MultipleConditionTest(){

    Item TTT = SILab2.createItem(null,"0876",530,0.1f);

    assertTrue(TTT.getPrice() > 300);
    assertTrue(TTT.getDiscount() > 0);
    assertEquals('0', TTT.getBarcode().charAt(0));

    Item TTF = SILab2.createItem(null,"6070",580,0.1f);

    assertTrue(TTF.getPrice() > 300);
    assertTrue(TTF.getDiscount() > 0);
    assertNotEquals('0', TTF.getBarcode().charAt(0));

    Item TF = SILab2.createItem(null,"8929",580,0);

    assertTrue(TF.getPrice() > 300);
    assertFalse(TF.getDiscount() > 0);

    Item F = SILab2.createItem(null,"8929",9,0);

    assertFalse(F.getPrice() > 300);
}
```

