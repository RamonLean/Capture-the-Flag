# Capture the Flag - CTF
CTF  - Enumerando, explorando vulnerabilidade, conseguindo shell e posteriormente acesso root.
<h2>Passos seguidos para realizar essa exploração.</h2>

1. Utilizar o **Netdiscover**
2. Escanear portas usando **Nmap**
3. Enumeração da aplicação web com **Dirb**
4. "Exploitando" a vulnerabilidade com código de execução remota **(Remote Code Execution)**
5. Conseguindo shel remota com Python.
6. Explorar permissão de acesso de arquivo e conseguindo acesso root.

# Passo 1: Encontrando o IP da máquina alvo.
  
  O primeiro passo é rodar o Netdiscover para verificar os IPs na rede.
  
  ![](Imagens/Imagem1.png)
  
  Como pode-se ver, o endreço IP do alvo é: 192.168.1.24
  
  # Passo 2:
  
  Após conseguir o endereço IP alvo, o primeiro passo é escanear as ṕortas abertas e avaliar os serviços em execução.
  Vamos utilizar o **Nmap**.
  
  ![](Imagens/Imagem2.png)
  
  **_comando utilizado nmap -A 192.168.1.24_**
  
  Pode-se ver acima que a porta 80 está aberta, com o serviço http rodando. Também é possível ver algumas informações sobre o sistema alvo.
  
  # Passo 3
  
  Analisando a aplicação rodando na porta 80.

  <h1 align="center">
  <img alt="" title="Imagem3" src="Imagens/Imagem3.png" />
  </h1>
  
  Como pode ser visto na imagem acima, o serviço rodando na porta 80 é a página padrão do **apache**.
  Vamos verificar se há algum arquivo que pode ser explorado. Para isso vamos rodar a ferramenta **Dirb** e enumerar os arquivos da aplicação.
  
  
  <h1 align="center">
  <img alt="" title="Imagem4" src="Imagens/Imagem3.png" />
  </h1>
  
  **_Comando usado dirb 192.168.1.24_**
  
  Alguns arquivos foram encontrados como resultado do scan. Eciste alguns arquivos que retornadram status 200 do servidor.
  Vamos verificar o arquivo **"robots.txt"** no navegador.
  
   <h1 align="center">
  <img alt="" title="Imagem5" src="Imagens/Imagem5.png" />
  </h1>
  
  Podemos ver que há uma entrada incomum no arquivo **_robots.txt_**. Verificando o arquivo no browser:
  
  <h1 align="center">
  <img alt="" title="Imagem6" src="Imagens/Imagem6.png" />
  </h1>
  
  Como podemos ver, **"sar2HTML"** é uma ferramenta  instalada na máquina alvo, sua versão está disponível.
  
  # Passo 4
  
  Agora temos a aplicação -"sar2HTML"- e sua versão. Fazendo uma pesquisa, podemos ver que a aplicação é vulnerável e possui um exploit disponível.
    
   <h1 align="center">
  <img alt="" title="Imagem7" src="Imagens/Imagem7.png" />
  </h1>
  
  O primeiro resultado no google nos leva ao **Exploit-DB**. A ferramenta que roda no sistema possui a vulnerabilidade de execução de código remoto. Podemos ver os detalhes do exploit abaixo:
  
   <h1 align="center">
  <img alt="" title="Imagem8" src="Imagens/Imagem8.png" />
  </h1>
  
  Podemos ver os parâmetros de utilização do exploit
  
Montando a URL para utilizar no alvo:  **Exploit <<<http:192.168.1.24/sar2HTML/index.php?plot=;>>>**

Tentando ler o arquivo **_"/etc/passwd"_** através da execução do comando na URL. O resultado pode ser vist abaixo:

   <h1 align="center">
  <img alt="" title="Imagem9" src="Imagens/Imagem9.png" />
  </h1>
  
  O exploit funcionouuu e retornou o conteúdo de "/etc/passwd".
  
  # Passo 5
  
  Agora podemos executar comandos no alvo através do navegador utilizando o exploit. Como o objetivo é conseguir o acesso root, vamos conseguir um acesso um shell reverso. Existem algumas possibilidades para isso:
        1. Podemos utilizar python and perl
        2. Podemos utilizar o metasploit pra criar o arquivo de conexão reversa, fazer download dele como o comando **wget** e executá-lo.
        3. Se a máquina alvo possuir o netcat, podemos utilizá-lo para criar a conexão reversa.
       
Tentando o terceiro método primeiro, para isso basta verificar se á máquina alvo possui o netcat. Podemos rodar o comando **"nc --help"**, se o comando retornar o resultado quer dizer que o netcat está disponível.

   <h1 align="center">
  <img alt="" title="Imagem10" src="Imagens/Imagem10.png" />
  </h1>
  
  Como podemos ver acima o comando não retornou nada. Então vamos tentar o primeiro método. Para isso basta verificar se existe python ou perl instalando no sistema. Testando python primerio
  
   <h1 align="center">
  <img alt="" title="Imagem9" src="Imagens/Imagem9.png" />
  </h1>
  
  Como podemos ver na imagem, python está disponível no alvo e pode ser usado para conexão reversa. 
