# Testen

## Refactor van de te testen klasse

1. Attribuut van de dummyklasse (dependency)

```
private Mikey mikey;
```

2. Constructor met initialisatie

```
public Class(){
  this(new Mikey());
}
```

3. Constructor met injectie

```
public Class(Mikey mikey){
  this.mikey = mikey;
}
```

4. Schrap eerdere creatie

```
// Mikey mikey = new Mikey();
```

## Opzet testklasse

1. Klasse annotatie

```
@ExtendWith(MockitoExtension.class)
public class Test {}
```

2. Mock declareren

```
@Mock
private Mikey mikeyMock;
```

3. Mock injecteren

```
@InjectMocks
private Class class;
```

## Verschillende soorten sources

1. CsvSource
   Meerdere PRIMITIEVE datatypes (meerdere params)

```
@CsvSource({"0.0, 1.0, 2.0"})
```

2. ValueSource
   Enkele PRIMITIEF datatype (enkele param)

```
@ValueSource(strings = {" ", "a", "A"})
```

3. MethodSource
   Objecten

```
private static Stream<Arguments> addFixture(){
 return Stream.of(
 Arguments.of(new int[]{5},5),
 Arguments.of(new int[]{2,3}, 5),
 Arguments.of(new int[]{5,6,9},5)
 );
}

@MethodSource("addFixture")
```

## Opbouw mockito test

1. Juiste source kiezen (zie hierboven) dmv te bepalen welke params er moeten meegegeven worden
2. Welke resultaten verwachten we? (meegeven met params)
3. Mock trainen

```
Mockito.when(mikeyMock.method()).thenReturn(param);
```

4. Te testen methode uitvoeren

```
Class class = class.method()
```

5. Params vergelijken met opgeslagen oplossing

```
Assertions.assertEquals(param, class);
```

6. Per getrainde mock een verify

```
Mockito.verify(mikeyMock).method();
```

## Opbouw JUnit test

Idem als hieroven, alleen zonder mockito implementatie!!
