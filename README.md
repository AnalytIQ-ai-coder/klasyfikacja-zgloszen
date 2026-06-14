# System klasyfikacji zgłoszeń uczelnianych

Prototyp systemu automatycznej klasyfikacji wiadomości/zgłoszeń studenckich przy użyciu polskiego modelu językowego **[Bielik-1.5B-v3.0-Instruct](https://huggingface.co/speakleash/Bielik-1.5B-v3.0-Instruct)**.

## Kategorie

| Kategoria | Opis |
|---|---|
| `stypendium` | Wnioski, wypłaty, wymagania, odwołania dotyczące stypendiów |
| `rekrutacja` | Przyjęcie na studia, dokumenty aplikacyjne, progi punktowe, IRK |
| `plan_zajec` | Harmonogram, sale, grupy, egzaminy, terminy zajęć i zaliczeń |
| `awaria_systemu` | Błędy techniczne w systemach IT uczelni (USOS, Moodle, VPN, poczta) |
| `sprawy_finansowe` | Czesne, faktury, opłaty, zwroty, konta bankowe uczelni |
| `inne` | Akademiki, stołówka, koła naukowe, regulaminy i inne |

## Techniki

Projekt porównuje dwa podejścia do klasyfikacji:

1. **Few-shot prompting** — w prompcie podajemy 5 przykładów każdej kategorii (30 łącznie); model klasyfikuje bez aktualizacji wag (czysta inferencja).
2. **QLoRA fine-tuning** — dotrenowujemy model na 144 przykładach (24/kat.) metodą LoRA w 4-bitowej kwantyzacji.

## Model

`speakleash/Bielik-1.5B-v3.0-Instruct` ładowany w **4-bitowej kwantyzacji (NF4)** przy użyciu `BitsAndBytesConfig`, co zmniejsza zużycie pamięci GPU ~4× względem FP16.

## Dane

- **180 własnych przykładów** — 30 na kategorię, po polsku, symulujące rzeczywiste zgłoszenia studenckie
- **`allegro/klej-polemo2-in`** — zewnętrzny zbiór recenzji użyty jako punkt odniesienia (test odporności na dane spoza domeny)

## Wymagania

- Python 3.10+
- GPU z CUDA (zalecane ≥ 8 GB VRAM; notebook przygotowany pod Google Colab T4)
- Konto Hugging Face z tokenem dostępu

## Instalacja

```bash
pip install -r requirements.txt
```

### Konfiguracja tokenu

**Opcja A — Google Colab Secrets (zalecane):**
W panelu bocznym Colab dodaj sekret o nazwie `HF_TOKEN` z wartością tokenu z [huggingface.co/settings/tokens](https://huggingface.co/settings/tokens).

**Opcja B — zmienna środowiskowa:**
```bash
cp .env.example .env
# uzupełnij HF_TOKEN w pliku .env
```

## Uruchomienie

Otwórz `Klasyfikacja_zgloszen.ipynb` w Google Colab lub lokalnie (z GPU) i uruchamiaj komórki kolejno.

## Struktura projektu

```
├── Klasyfikacja_zgloszen.ipynb   # główny notebook
├── requirements.txt               # zależności Python
├── .env.example                   # szablon zmiennych środowiskowych
└── .gitignore
```
