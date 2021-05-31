# Swap

## O que é swap?
Swap é uma área de troca (memória virtual) e funciona como uma extensão da memória RAM, mas fica armazenada no disco. Pode ser considerada como uma memória de fuga, pois quando esgotar os recursos de memória RAM no sistema o swap será usado para aliviar a carga e não travar o sistema.
Swap pode ser uma partição no disco ou um arquivo (swap file)

Importante: lembre-se que o swap é bem mais lento que a memória RAM. Se você fizer muito swap em um sistema você vai lidar com uma grande de uma lentidão!
Mesmo em casos onde o swap é feito em um SSD


## Quando a swap é usada?
  Quando a memória ram enche;<br>
  Quando o kernel usa um programa que detecta blocos, conhecidos como paginas de memória com conteúdo em RAM que não foi usado recentemente;<br>
  Quando precisa de mais memória esse programa pode ser alocado na swap, se precisar das páginas tirar da swap e usar a memória.


## O que acontece se o sistema fica sem ram e sem swap?
  Acontece um evento chamado THRASHING, onde o sistema fica absurdamente lento


# Linux consumindo muita memória RAM?

## O que está acontecendo?
  O Linux está pegando emprestado a memória não utilizada para o cache de disco. Isso faz parecer que você está com pouca memória, mas não está!

## Por que está fazendo isso?
  O cache de disco torna o sistema muito mais rápido e responsivo! Não há desvantagens, exceto para iniciantes confusos. Ele não tira a memória dos aplicativos de forma alguma, nunca!

## E se eu quiser executar mais aplicativos ou serviços?
  O cache de disco torna o sistema muito mais rápido e responsivo! Não há desvantagens, exceto para iniciantes confusos. Ele não tira a memória dos aplicativos de forma alguma, nunca!

## Eu preciso de mais swap?
  Não, o cache de disco apenas pega emprestado a memória RAM que os aplicativos não desejam no momento. Não usará swap. Se os aplicativos quiserem mais memória, eles simplesmente a retiram do cache de disco. Eles não vão começar a trocar.

## Como faço para impedir que o Linux faça isso?
  Você não pode desativar o cache de disco. A única razão pela qual alguém deseja desabilitar o cache de disco é porque eles pensam que isso tira a memória de seus aplicativos, o que não acontece! O cache de disco faz com que os aplicativos sejam carregados com mais rapidez e suavidade, mas NUNCA tira memória deles! Portanto, não há absolutamente nenhuma razão para desativá-lo!<br>
  Se, no entanto, você precisar limpar um pouco de RAM rapidamente para contornar outro problema, como um mau comportamento de VM, você pode forçar o linux a não destrutiva eliminar caches de forma usando ```echo 3 | sudo tee /proc/sys/vm/drop_caches```.

## Por que top e free dizem que todo o meu ram é usado se não é?
  Esta é apenas uma diferença de terminologia. Você e o Linux concordam que a memória ocupada pelos aplicativos é "usada", enquanto a memória que não é usada para nada é   "livre".

  ### Mas como você conta a memória que é usada atualmente para algo, mas ainda pode ser disponibilizada para aplicativos?
   Você pode contar essa memória como "livre" e / ou "disponível". Em vez disso, o Linux o conta como "usado", mas também "disponível":

  <table style="width:100%">
    <tr>
      <th>Memória que é</th>
      <th>Você chamaria</th>
      <th>Linux chama isso</th>
    </tr>
    <tr>
      <td>usada por aplicativos</td>
      <td>Usada</td>
      <td>Usada</td>
    </tr>
    <tr>
      <td>usada, mas pode ser disponibilizada</td>
      <td>Livre (ou disponível)</td>
      <td>Livre (e disponível)</td>
    </tr>
    <tr>
      <td>não é usada para nada</td>
      <td>Livre</td>
      <td>Livre</td>
    </tr>
  </table>

   >Esse "algo" é (aproximadamente) o que o top e o free chamam de "buffers" e "cached". Como a terminologia sua e a do Linux são diferentes, você pode pensar que está com pouca memória RAM quando não está.

## Como posso ver a quantidade de RAM livre que realmente tenho?
  Para ver quanta RAM seus aplicativos podem usar sem trocar, execute free -me olhe para a coluna "available":
  ```bash
    $ free -m
                  total        used        free      shared  buff/cache   available
    Mem:           1504        1491          13           0         855      792
    Swap:          2047           6        2041
  ```

  Esta é a sua resposta no MiB. Se você ingenuamente olhar para "usado" e "livre", você pensará que sua RAM está 99% cheio, quando na verdade está apenas 47%!
  Para uma descrição mais detalhada e técnica do que o Linux conta como "disponível", consulte o [commit](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=34e431b0ae398fc54ea69ff85ec700722c9da773) que adicionou o campo.

## Quando devo começar a me preocupar?

### Um sistema Linux saudável com memória mais do que suficiente irá, depois de funcionar por um tempo, mostrará o seguinte comportamento esperado e inofensivo:
  * free: a memória está perto de 0;
  * used: a memória está perto de total;
  * avaliable: memória tem espaço suficiente (digamos, 20%+ do total);
  * swap used: não muda.

### Sinais alerta de uma situação genuína de baixa memória que você pode querer examinar:
  * avaliable: memória está perto de zero;
  * swap used: aumenta ou flutua;
  * dmesg | grep oom-killer: mostra o OutOfMemory-killer em ação

## Como posso verificar essas coisas?
  Consulte esta [página](https://www.linuxatemyram.com/play.html) para obter mais detalhes e como você pode experimentar o cache de disco para mostrar os efeitos descritos aqui. Poucas coisas o fazem apreciar mais o cache de disco do que medir uma aceleração da ordem de magnitude em seu próprio hardware!
