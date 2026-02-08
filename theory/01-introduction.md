# Introduzione

## Prerequisiti

Avere una conoscenza base del linguaggio Rust.

## La famiglia di schede ESP32

Se sei alle prime armi e stai cercando una scheda ESP32 da acquistare, potresti sentirti sopraffatto dalle numerose opzioni e varianti disponibili.

In questa lezione, esplorerai queste varianti.

### Varianti System on Chip (SoC)

La famiglia di schede ESP32 è composta dalle seguenti varianti SoC:

- ESP32
- ESP32 S Series (ESP32-S2, ESP32-S3)
- ESP32 C Series (ESP32-C3, ESP32-C5, ESP32-C6)
- ESP32 H Series (ESP32-H2)
- ESP32 P Series (ESP32-P4)

Puoi trovare un elenco completo di questi modelli e le relative specifiche nella [pagina di confronto prodotti](https://products.espressif.com/#/product-comparison) di Espressif.

### System on Chip (SoC) vs Module vs Development Board (DevKit)

L'ESP32 è disponibile in tre diverse versioni: System on Chip (SoC), Module e Development Board (DevKit).
Ognuna di esse ha uno scopo specifico ed è adatta a diverse fasi di sviluppo o integrazione.

![](imgs/devkit-module-soc.png)

#### System on Chip (SoC)

Il SoC è il cuore e la forma più fondamentale dell'ESP32.
Un System on Chip integra i componenti essenziali di un sistema elettronico o di un computer in un singolo circuito integrato (IC).
Nel caso del SoC ESP32, include:

- **CPU (Central Processing Unit)**
- **Wi-Fi e Bluetooth**
- **ROM e SRAM**
- **Periferiche aggiuntive**

> [!TIP]
> I SoC sono principalmente progettati per l'integrazione in progetti hardware personalizzati.
> Sono ideali per i produttori che desiderano integrare le funzionalità ESP32 nei propri prodotti.
> Tuttavia, poiché il SoC di per sé non è pre-certificato per la conformità normativa, è necessario ottenere certificazioni (ad esempio, FCC, CE, ...) quando si progetta hardware personalizzato per la comunicazione wireless.

#### Module

I moduli si basano sul SoC e offrono una soluzione più intuitiva e pronta all'uso.
Esempi comuni di moduli ESP32 includono le popolari **WROOM** e **WROVER**.
Ad esempio, l'**ESP32-WROOM-32** è ampiamente utilizzato e riconoscibile come componente quadrato con involucro metallico su molte schede di sviluppo.

Perché scegliere un modulo?

- **Pre-certificazione**: i moduli sono pre-certificati per la conformità normativa, con conseguente risparmio di tempo e fatica.
- **Componenti integrati**: includono antenne PCB, un oscillatore al cristallo e un chip di memoria flash.
- **Design schermato**: i moduli sono dotati di uno schermo EMI per ridurre le interferenze elettromagnetiche.

> [!TIP]
> I moduli sono progettati per l'integrazione in PCB personalizzati e sono perfetti per applicazioni che richiedono meno componenti esterni e meno sforzi rispetto all'utilizzo di un SoC grezzo.

#### Development Board (DevKit)

Le schede di sviluppo semplificano l'uso dei moduli ESP32 per la prototipazione e lo sviluppo.
Includono componenti aggiuntivi per rendere il modulo ESP32 più facile da usare sia per i principianti che per gli sviluppatori esperti.

Caratteristiche principali delle schede di sviluppo:

- **Interfaccia USB**: consente una facile programmazione e debug
- **Regolatori di tensione**: garantiscono un funzionamento stabile per l'ESP32
- **Breakout dei pin**: rendono accessibili i pin dell'ESP32 per il collegamento di componenti esterni come sensori e display
- **Pulsanti di avvio e reset**: forniscono il controllo sulle operazioni del modulo

> [!TIP]
> I DevKit sono ideali per la prototipazione rapida e la sperimentazione.
> Esempi popolari includono **ESP32 DevKit v1** ed **ESP32-S3-DevKitC**, ampiamente utilizzati da sviluppatori e hobbisti.

### Perché usi la scheda **ESP32 DevKit v1**?

Nonostante l'esistenza di tante varianti, la scheda più utilizzata per imparare e prototipare velocemente rimane l'**ESP32 DevKit v1**.

<p align="center">
    <img src="imgs/esp32-devkitv1.jpg" height="300" />
</p>

L'ESP32 è un processore dual-core a 32 bit dotato di Wi-Fi e Bluetooth, perfetto per creare applicazioni IoT wireless.

Di seguito sono riportate le specifiche di base per l'ESP32:

- Processore: Xtensa LX6 a 32 bit
- Numero di core: 2
- Frequenza di clock: 240 MHz
- Memoria flash: 4 MB
- ROM: 448 KB (programmi di sola lettura essenziali per il funzionamento dell'ESP32)
- SRAM: 520 KB (per memorizzare dati e istruzioni)
- ADC: ADC SAR a 12 bit, 18 canali, 6 pin di ingresso
- UART: 3
- SPI: 2
- I2C: 3
- Wi-Fi: IEEE 802.11 b/g/n/e/i (802.11n fino a 150 Mbps)
- Bluetooth: v4.2 BR/EDR e Bluetooth Low Energy (BLE)
- Tensione di funzionamento: 2,3-3,6 V
- Deep Sleep: 100 µA

## ESP32 Pinout

![](imgs/ESP32-DevKit-V1-Pinout-Diagram.png)

## Development Environment

La [documentazione ufficiale](https://docs.espressif.com/projects/rust/book/getting-started/index.html) fornisce istruzioni di configurazione più complete.
Tuttavia, illustrerò brevemente gli strumenti essenziali e la configurazione necessaria per questo corso.

> [!WARNING]
> In caso di problemi, consulta la documentazione ufficiale.

### Comandi

Di seguito trovi la lista dei comandi per scaricare tutto il necessario per sviluppare il firmware di questo corso:

```bash
# Utile per velocizzare l'installazione dei tool di sviluppo; non influisce sul processo di build del firmware.
cargo install cargo-binstall

# `espflash` è tool di flashing seriale, basato su `esptool.py`, per SoC e moduli Espressif.
# Questo è lo strumento utilizzato per inserire il tuo codice nella scheda ESP32 ed eseguirlo.
cargo binstall espflash
# espflash --version

# Il tool `esp-generate` viene utilizzato per creare applicazioni `no_std`.
# Attualmente supporta ESP32, ESP32-C2/C3/C6, ESP32-H2 ed ESP32-S2/S3.
cargo binstall esp-generate --locked
# Sostituisci `PROJECT_NAME` e `esp32` con la scheda ESP32 utilizzata e col nome del progetto.
# esp-generate --chip esp32 PROJECT_NAME

# Avrai anche bisogno di `espup` per installare le toolchain necessarie: Xtensa e RISC-V.
cargo binstall espup
espup install

# Configurazione globale dell'ESP toolchain
# Nota: il file di configurazione della shell cambia in base al tipo di shell.
#       Sulle distribuzioni Linux si usa il file `~/.bashrc`.
echo '. ~/export-esp.sh' >> ~/.zshrc

# Controlla se tutto è andato a buon fine.
which xtensa-esp32-elf-gcc
```

### Hello Embedded World!

Prima di addentrarti nella teoria e nei concetti di funzionamento, passa direttamente all'azione.
Ti mostro come generare un firmware, scritto in linguaggio Rust, per accendere il built-in LED (`GPIO2`) dell'**ESP32 DevKit v1**.

Per creare un nuovo progetto usa il comando:

```bash
esp-generate --chip esp32 esp32-quick
```

Si aprirà la seguente finestra:

![](imgs/esp-generate.png)

Per ora non è necessario selezionare alcuna opzione, quindi salva premendo <kbd>s</kbd> sulla tastiera.

Infine ti verrà posta la seguente domanda:

```bash
Do you want to run `cargo install esp-config --features=tui --locked` now? [y/N]
```

Premi <kbd>N</kbd> sulla tastiera.

Successivamente entra nella cartella del progetto usando il comando:

```bash
cd esp32-quick
```

Ora apri il file `src/bin/main.rs`, al cui interno troverai del codice generato automaticamente.
Bene, sostituisci tutto il codice con il seguente:

```rust
#![no_std]
#![no_main]
#![deny(
    clippy::mem_forget,
    reason = "mem::forget is generally not safe to do with esp_hal types, especially those \
    holding buffers for the duration of a data transfer."
)]

use esp_hal::clock::CpuClock;
use esp_hal::gpio::{Level, Output, OutputConfig};
use esp_hal::main;
use esp_hal::time::{Duration, Instant};

#[panic_handler]
fn panic(_: &core::panic::PanicInfo) -> ! {
    loop {}
}

// This creates a default app-descriptor required by the esp-idf bootloader.
// For more information see: <https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/system/app_image_format.html#application-description>
esp_bootloader_esp_idf::esp_app_desc!();

#[main]
fn main() -> ! {

    let config = esp_hal::Config::default().with_cpu_clock(CpuClock::max());
    let peripherals = esp_hal::init(config);

    let mut led = Output::new(peripherals.GPIO2, Level::High, OutputConfig::default());

    loop {
        led.toggle();

        blocking_delay(Duration::from_millis(500));
    }

}

fn blocking_delay(duration: Duration) {
    let delay_start = Instant::now();
    while delay_start.elapsed() < duration {}
}
```

> [!WARNING]
> Per ora non preoccuparti del codice: spiegherò tutto nella prossima lezione.
> Quello che voglio mostrarti in questa lezione è la semplicità nell'uso del linguaggio Rust nel mondo Embedded.

A questo punto puoi caricare il codice sulla tua ESP32 usando il seguente comando:

```bash
cargo run
```

E dopo qualche secondo vedrai il LED integrato (`GPIO2`) lampeggiare.

## Livelli di astrazione

Quando lavori con Rust nei sistemi embedded, ti imbatti spesso in termini come **PAC**, **HAL** e **BSP**.
Questi sono i diversi livelli che aiutano a interagire con l'hardware.
Ogni livello offre un diverso equilibrio tra flessibilità e facilità d'uso.

Partiamo dal livello più alto di astrazione fino a quello più basso:

![](imgs/abstraction-layers.png)

### BSP (Board Support Package)

Un **BSP**, noto anche come **Board Support Crate** in Rust, è progettato su misura per specifiche schede di sviluppo.
Combina l'**HAL** con configurazioni specifiche per la scheda, fornendo interfacce pronte all'uso per componenti integrati come LED, pulsanti e sensori.
Ciò consente agli sviluppatori di concentrarsi sulla logica applicativa anziché occuparsi di dettagli hardware di basso livello.
Poiché non esiste un **BSP** specifico per **ESP32 DevKit v1**, in questo corso non userai questo approccio.

### HAL (Hardware Abstraction Layer)

L'**HAL** si trova appena sotto il livello **BSP**.
Se lavori con schede come **Raspberry Pi Pico** o basate su ESP32, utilizzerai principalmente il livello HAL.

L'HAL si basa sul **PAC** e fornisce interfacce più semplici e di livello superiore per le periferiche del microcontrollore.
Invece di gestire direttamente i registri di basso livello, gli HAL offrono metodi e caratteristiche che semplificano attività come l'impostazione di timer, la configurazione della comunicazione seriale o il controllo dei pin GPIO.

Gli HAL per i microcontrollori implementano solitamente i traits [`embedded-hal`](https://github.com/rust-embedded/embedded-hal), ovvero interfacce standard indipendenti dalla piattaforma per periferiche come GPIO, SPI, I2C e UART.
Questo semplifica la scrittura di driver e librerie compatibili con hardware diversi, purché utilizzino un HAL compatibile.

In questo corso userai il crate [`esp-hal`](https://github.com/esp-rs/esp-hal).

Lo stesso HAL può essere usato con altre varianti di ESP32.
Per passare a una famiglia ESP32 diversa, è sufficiente aggiornare il flag delle funzionalità nel file `Cargo.toml`.

> [!NOTE]
> I livelli sottostanti l'HAL sono raramente utilizzati direttamente.
> Nella maggior parte dei casi, l'accesso al PAC avviene tramite l'HAL, non autonomamente.
> A meno che non si lavori con un chip che non dispone di un HAL, di solito non è necessario interagire direttamente con i livelli inferiori.
> In questo corso ti concentrerai sul livello HAL.

### PAC (Peripheral Access Crate)

I **PAC** rappresentano il livello più basso di astrazione.
Sono crate generati automaticamente che forniscono un accesso type-safe alle periferiche di un microcontrollore.
Questi crate vengono in genere generati dal file **SVD (System View Description)** del produttore utilizzando strumenti come [`svd2rust`](https://github.com/rust-embedded/svd2rust).
I PAC offrono un modo strutturato e sicuro per interagire direttamente con i registri hardware.

### Crate di microarchitettura

Si trova accanto al PAC nella gerarchia di astrazione.
Questi crate sono specifici per l'architettura del core del processore (ad esempio, ARM Cortex o Xtensa) utilizzata nel microcontrollore.
Forniscono accesso di basso livello alle funzionalità del core e alle periferiche interne condivise.

Per ESP32, si tratta dei crate [`xtensa-lx`](https://github.com/esp-rs/xtensa-lx) e [`xtensa-lx-rt`](https://github.com/esp-rs/xtensa-lx-rt).
Questi gestiscono operazioni come l'abilitazione o la disabilitazione degli interrupt e l'accesso ai timer interni.
Per i microcontrollori basati su ARM Cortex, come quelli delle famiglie STM32, nRF o RP2040, i crate equivalenti sono [`cortex-m`](https://github.com/rust-embedded/cortex-m) e [`cortex-m-rt`](https://github.com/rust-embedded/cortex-m-rt).
Questi crate di microarchitettura costituiscono la base su cui si basano HAL e BSP di livello superiore, garantendo compatibilità e riutilizzo tra chip che condividono lo stesso core.

### Raw MMIO

Il **Raw MMIO (Memory-Mapped IO)** implica l'utilizzo diretto dei registri hardware, leggendo e scrivendo su indirizzi di memoria specifici.
Questo approccio rispecchia la tradizionale manipolazione dei registri in stile C e richiede l'uso di blocchi `unsafe` in Rust a causa dei potenziali rischi connessi.
In questo corso non approfondirai questo aspetto: nella pratica è raro vederlo usato.

### Risorse esterne

Se vuoi approfondire i concetti citati sopra, qui trovi le risorse (tutte da Comprehensive Rust).

**Consigliate per questo corso (userai soprattutto HAL/PAC):**
- [HAL (Hardware Abstraction Layer)](https://google.github.io/comprehensive-rust/bare-metal/microcontrollers/hals.html) — come si lavora con periferiche senza toccare registri.
- [PAC (Peripheral Access Crate)](https://google.github.io/comprehensive-rust/bare-metal/microcontrollers/pacs.html) — accesso type-safe ai registri: utile per capire cosa c'è "sotto" l'HAL.

**Extra (per curiosità/contesto):**
- [BSP (Board Support Package)](https://google.github.io/comprehensive-rust/bare-metal/microcontrollers/board-support.html) — esempio di crate pensato per una specifica board.
- [Raw MMIO](https://google.github.io/comprehensive-rust/bare-metal/microcontrollers/mmio.html) — accesso diretto a indirizzi di memoria (approccio più basso livello).
