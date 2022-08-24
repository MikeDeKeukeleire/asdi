# Java

## Collections & streams

1. Collection -> map

```
// 1 object
collect(Collectors.toMap(sleutel, waarde));

// meerdere objecten (list)
collect(Collectors.groupingBy(sleutel, waarde));
```

**Wanneer de sleutel/waarde het object zelf is:**

```
Function.identity()
```

**Wanneer je bij groupingby een list van objecten wil meegeven**

```
Collectors.toList() // dit is de default!
```

2. Sorteren van een map

```
collect(Collectors.groupingBy(Bier::getSoort, TreeMap::new, Collectors.toList()));
```

**Hier moet de laatste waarde wél meegegeven worden!**

3. Tellen van objecten om in een waarde/sleutel te steken

```
collect(Collectors.groupingBy(Bier::getSoort, TreeMap::new, Collectors.counting()));
```

### 4 belangrijke methodes bij een map!

a) keySet() -> enkel de sleutels

b) values() -> enkel de waarden

c) forEach() -> enkel de map overlopen

d) entrySet() -> verwerkingen op de map uitvoeren

d.1) getKey()

d.2) getValue()

d.3) setValue()

4. Map -> String

```
map.entrySet().stream().map(e -> String.format("%s = %s", e.getKey(), e.getValue()))
				.collect(Collectors.joining("\n"));
```

5. Objecten filteren uit een list

```
lijst.stream().filter(e -> e.getX() > param)
.map(Class::getX).collect(Collectors.toList());
```

6. Lijst sorteren

```
lijst.sort(Comparator.comparing(Class::getX)
				.thenComparing(Comparator.comparing(Class::getY).reversed()));
```

**Reversed slaat op Comparator! & sort() gaat een bestaande lijst modificeren, sorted() maakt een nieuwe aan!**

7. **Flatmap!**

```
sportersLijst.stream().map(Sporter::getReductiebonLijst).flatMap(Collection::stream).filter(bon -> kortingspercentage.contains(bon.getPercentage())).collect(Collectors.toList());
```

8. Unieke objecten teruggeven

```
lijst.stream().map(Class::getX).distinct().sorted().collect(Collectors.toList());
```

9. 1 object teruggeven

```
.findAny().orElse(null);
```

10. Verwijderen

```
.removeIf(lambda)
```

## Netwerken

1. TCP verbinding met localhost + versturen van string

```
	try (Socket socket = new Socket(InetAddress.getLocalHost(), 23400)){
			Formatter output = new Formatter(socket.getOutputStream());
			output.format("%s, %s, %s", "","","");
			output.flush();
		} catch (Exception e) {
			e.printStackTrace();
		}
```

2. Lijst met clients

```
private List<InetAddress> clients = new ArrayList<>();
```

3. Clients toevoegen

```
clients.add(InetAddress.getLocalHost());
```

4. TCP verbinding + ontvangen van string

```
try (ServerSocket ss = new ServerSocket(23400, 5)){
				while(true) {
					System.out.println("wacht op aanbieder");
					Socket s = ss.accept();
					Scanner sockInput = new Scanner(s.getInputStream());
					koopjes.add(new Auto(sockInput.next(), sockInput.next(), sockInput.next()));
					System.out.println("added");
					s.close();
				}
			} catch (Exception e) {
				e.printStackTrace();
			}
```

5. UDP verbinding + versturen

```
		try (DatagramSocket dgSocket = new DatagramSocket()){
			DatagramPacket dgPakket = new DatagramPacket(koopjes.toString().getBytes(),koopjes.toString().getBytes().length);
			clients.forEach(adres -> {
				System.out.println("versturende");
				try {
					dgPakket.setPort(9999);
					dgPakket.setAddress(adres);
					dgSocket.send(dgPakket);
				} catch (Exception e) {
					e.printStackTrace();
				}
			});
		} catch (Exception e) {
			e.printStackTrace();
		}
```

6. UDP verbinding + ontvangen

```
try(DatagramSocket datagramSocket = new DatagramSocket(9999)){
			byte[] buffer = new byte[100];
			DatagramPacket dp = new DatagramPacket(buffer,buffer.length);
			datagramSocket.receive(dp);
			System.out.println(new String(dp.getData(),dp.getOffset(),dp.getLength()
					));
		}catch(IOException io) {
			io.printStackTrace();
		}
```
