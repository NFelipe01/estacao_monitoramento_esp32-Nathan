# Estação de Monitoramento IoT — ESP32

Estação de monitoramento baseada em ESP32 que disponibiliza uma página Web
(via HTTP convencional, sem WebSocket ou MQTT) para:

- Visualizar a temperatura e umidade medidas pelo sensor **DHT11**
- Visualizar o nível de luminosidade obtido pelo **LDR**
- Visualizar o estado dos 4 **botões** da placa
- Acionar e desligar o **relé**
- Controlar a cor do **LED RGB**

Placa utilizada: *Plataforma de Aprendizagem de Circuitos Embarcados* (ESP32-DEVKITC-32D).

## Hardware necessário

- Placa ESP32-DEVKITC-32D (Plataforma de Aprendizagem de Circuitos Embarcados)
- Cabo USB
- Rede Wi-Fi 2.4 GHz

## Pinagem utilizada

| Componente        | GPIO |
|-------------------|------|
| DHT11 (DATA)      | 33   |
| LDR (VN)          | 39   |
| Botão SW1         | 4    |
| Botão SW2         | 0    |
| Botão SW3         | 2    |
| Botão SW4         | 15   |
| Relé              | 13   |
| LED RGB — R       | 27   |
| LED RGB — G       | 26   |
| LED RGB — B       | 25   |

## Pré-requisitos de software

1. **Arduino IDE** (versão 2.3.10 recomendada).
2. **Suporte à placa ESP32** instalado em Arquivo/File > Preferências, adicionando a URL do gerenciador de placas da Espressif e instalando "esp32" pelo Gerenciador de Placas.
3. Biblioteca **"DHT sensor library"** (Adafruit), instalada pelo Gerenciador de Bibliotecas — a instalação vai pedir também a dependência **"Adafruit Unified Sensor"**, aceite instalar as duas.

## Como compilar

1. Abra o arquivo `estacao_monitoramento.ino` no Arduino IDE.
2. Em **Ferramentas > Placa**, selecione **ESP32 Dev Module**.
3. Em **Ferramentas > Porta**, selecione a porta serial onde o ESP32 está conectado.
4. No código, edite as credenciais de Wi-Fi se necessário:
   ```cpp
   const char* ssid = "SEU_WIFI";
   const char* password = "SUA_SENHA";
   ```
5. Clique em **Verificar/Compilar** (ícone ✓) para checar se compila sem erros.

## Como executar

1. Com o código compilado sem erros, clique em **Carregar/Upload** (ícone →) para gravar na placa.
2. Abra o **Monitor Serial** (115200 baud).
3. Pressione o botão **EN/RESET** da placa, se necessário, para reiniciar o sketch.
4. Aguarde a mensagem de conexão Wi-Fi e anote o **endereço IP** exibido, por exemplo:
   ```
   Endereco IP: 192.168.00.000
   ```
5. Em um dispositivo conectado à **mesma rede Wi-Fi**, abra o navegador e acesse esse IP (ex: `http://192.168.00.000`).
6. A página exibirá os dados dos sensores (atualizados a cada 2 segundos), os indicadores dos botões, o controle do relé e os sliders de cor do LED RGB.

## Configuração de hardware necessária na placa

- A chave seletora **SW5** deve estar com as 4 posições em **OFF (Botão)** para que os botões SW1–SW4 funcionem (na posição ON, os mesmos pinos acionam os LEDs D1–D4 em vez de ler os botões).
- O **jumper JP1** deve estar instalado para a referência de tensão dos botões.

## Rotas HTTP disponíveis

| Rota                     | Método | Descrição                                             |
|---------------------------|--------|--------------------------------------------------------|
| `/`                        | GET    | Página HTML principal                                  |
| `/dados`                   | GET    | Retorna um JSON com temperatura, umidade, luminosidade, estado dos botões, do relé e do RGB |
| `/rele?estado=1` ou `0`    | GET    | Liga (`1`) ou desliga (`0`) o relé                      |
| `/rgb?r=&g=&b=`            | GET    | Define a cor do LED RGB (valores de 0 a 255 por canal)  |

## Autores

- Acadêmico: Nathan da Silva Felipe
- Orientador: Vagner da Silva Rodrigues
