Dans le mail, il y a un base64, ce base64 est une archive ZIP qui contient un payload java (PDF/Main.class):

```java
package PDF;

public class Main {
   public static void main(String[] var0) {
      String var1 = "VAAA{C0ylty0";
      String var2 = "ty0g_S1y3_X4";
      String var3 = "a_O3_Qnatre0hf}";
      System.out.println(var1 + var2 + var3);
   }
}
```

ce qui donne au final:

>VAAA{C0ylty0ty0g_S1y3_X4a_O3_Qnatre0hf}

Comme l'indice du PDF annonçait, il fallait faire un ROT13, le flag final est donc:

>INNN{P0lygl0gl0t_F1l3_K4n_B3_Danger0us}
