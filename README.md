<div align="center">

# 🧭 Quotes
### *a little read, to start great*

![Swift](https://img.shields.io/badge/Swift-5.9-F05138?style=for-the-badge&logo=swift&logoColor=white)
![iOS](https://img.shields.io/badge/iOS-18%2B-000000?style=for-the-badge&logo=apple&logoColor=white)
![SwiftUI](https://img.shields.io/badge/SwiftUI-✦-0070F3?style=for-the-badge)
![FoundationModels](https://img.shields.io/badge/Apple_FoundationModels-AI-A2AAAD?style=for-the-badge&logo=apple&logoColor=white)
![License](https://img.shields.io/badge/license-MIT-orange?style=for-the-badge)

<br/>

> *"In your heart lies a compass pointing to treasures not yet imagined, waiting for the brave."*

<br/>

</div>

---

## 📖 About

**Quotes** is a minimal, single-screen iOS app that greets you each day with a fresh AI-generated inspirational quote. Beyond the quote itself, a built-in **Quote Companion** lets you dive deeper — ask questions, reflect on the meaning, or simply enjoy a thoughtful exchange.

This project started as a complete rebuild of [**Fortu**](https://apps.apple.com/it/app/fortu-daily-fortune/id6753077218?l=en-GB), my first iOS app, developed as part of **Challenge #2 (Americano)** at the [Apple Developer Academy](https://www.developeracademy.unina.it) 2025–26. The rebuild was driven by a desire to go deeper on SwiftUI design, fluid animations, haptics, and on-device AI through Apple's `FoundationModels` framework.

---

## ✨ Features

| Feature | Description |
|---|---|
| 🗓 **Daily Quotes** | A new AI-generated quote every day via a dedicated `QuoteProvider` |
| ✦ **Quote Companion** | Tap the sparkles button to open a chat modal — the AI explains the quote and responds to your questions |
| 🌅 **Time-Based Gradients** | The background evolves throughout the day — morning blues, afternoon teals, evening purples, and deep night indigos |
| 🃏 **Interactive Quote Card** | Drag the card to trigger a 3D tilt effect with spring physics — it feels alive |
| 🧭 **Spin the Compass** | Flick the compass icon and watch it spin with momentum and spring damping |
| ❤️ **Favorites** | Save quotes you love — the foundation for personalized AI generation |
| 📜 **History View** | Browse all past quotes at a glance |
| 📤 **Share** | Share your daily quote directly from the app |
| 🔊 **Sound & Haptics** | Subtle tactile and audio feedback throughout the experience |

---

## 🎨 Design Philosophy

Quotes+ is built around three principles:

- **Single-screen** — no navigation clutter, just you and your quote
- **Appealing UI** — gradients, animations, and depth that feel native and alive
- **Simplicity** — every element earns its place

---

## 🛠 Technical Highlights

### 🌈 Time-Based Background

The `BackgroundGradientEngine` reads the current hour and shifts the color palette accordingly:

```swift
class BackgroundGradientEngine: ObservableObject {
    @Published var colors: [Color] = []

    func updateGradient() {
        let hour = Calendar.current.component(.hour, from: Date())
        switch hour {
        case 5..<12:
            colors = [.blue.opacity(0.3), .blue, .cyan.opacity(0.3)]
        case 12..<19:
            colors = [.blue.opacity(0.5), .blue.opacity(0.8), .teal.opacity(0.5)]
        case 19..<22:
            colors = [.purple.opacity(0.6), .pink.opacity(0.7), .purple.opacity(0.4)]
        default:
            colors = [.indigo.opacity(0.9), .purple.opacity(0.95), .black.opacity(1)]
        }
    }
}
```

### 🃏 Interactive Card Modifier

The quote card responds to drag gestures with a 3D rotation, subtle offset, and a scale pop — all driven by an `.interactiveSpring`:

```swift
struct InteractiveCardModifier: ViewModifier {
    let isInteractive: Bool
    @Binding var dragAmount: CGSize
    @Binding var isDragging: Bool

    func body(content: Content) -> some View {
        content
            .rotation3DEffect(
                .degrees(max(min(Double((dragAmount.width + dragAmount.height) / 25), 4), -4)),
                axis: (x: 1, y: 1, z: 0)
            )
            .offset(x: dragAmount.width / 20, y: dragAmount.height / 20)
            .scaleEffect(isDragging ? 1.01 : 1)
            .animation(.interactiveSpring(response: 0.35, dampingFraction: 0.6, blendDuration: 0.6),
                       value: dragAmount)
    }
}
```

### 🧭 Spinnable Compass

The compass icon can be spun with a drag gesture. It carries momentum and springs back naturally using `interpolatingSpring`:

```swift
.onEnded { value in
    let velocity = value.predictedEndTranslation.width - value.translation.width
    withAnimation(.interpolatingSpring(stiffness: 8, damping: 3)) {
        rotation += Double(velocity) * 1.5
    }
}
```

### 🤖 AI — Apple FoundationModels

Both quote generation and the Quote Companion are powered entirely on-device using Apple's `FoundationModels` framework — no API keys, no server, full privacy.

- **`QuoteProvider`** — generates a unique quote each day
- **`ChatSessionManager`** — manages the multi-turn conversation session in the Quote Companion

---

## 🗺 Architecture

```
CompassQuotes/
├── Models/
│   ├── QuoteProvider.swift          # LLM-powered daily quote generation
│   └── ChatSessionManager.swift     # Multi-turn chat session for Quote Companion
├── Views/
│   ├── ContentView.swift            # Main single-screen layout
│   ├── QuoteCardView.swift          # Interactive quote card
│   ├── ChatView.swift               # Quote Companion modal
│   ├── HistoryView.swift            # Past quotes list
│   └── FavoritesView.swift          # Saved quotes
├── Modifiers/
│   ├── InteractiveCardModifier.swift
│   └── BackgroundGradientEngine.swift
└── Components/
    └── AsteriskView.swift           # Spinnable compass icon
```

---

## 🚀 Getting Started

1. **Clone the repo**
   ```bash
   git clone https://github.com/haartt/CompassQuotes.git
   cd CompassQuotes
   ```

2. **Open in Xcode**
   ```bash
   open CompassQuotes.xcodeproj
   ```

3. **Run on a physical device** (required for `FoundationModels` on-device AI)

> ⚠️ Apple's `FoundationModels` framework requires iOS 18+ and a compatible device with Apple Intelligence enabled.

---

## 🔮 Next Steps

- [ ] **Favorites-based quote generation** — tailor the AI output to what resonates with you
- [ ] **Shaders on QuoteCards** — GPU-powered visual effects on the card surface
- [ ] **UI & UX improvements** — continued refinement of transitions and layout
- [ ] **Prompt engineering** — sharper, more evocative quotes
- [ ] **Haptics** — richer tactile feedback patterns

---

## 📚 Learning Goals

This project was built to explore and deepen skills in:

| Area | Focus |
|---|---|
| **UI Design** | Gradients, time-based color systems, animations, depth |
| **Interactive Dynamics** | Spring physics, drag gestures, 3D transforms |
| **LLMs** | On-device AI with Apple FoundationModels |
| **Haptics** | Tactile feedback integrated with interactions |

---

## 🎓 Context

This app was created as part of **Challenge #2 — Americano** at the **Apple Developer Academy 2025–26**, a program at the University of Naples Federico II. The challenge brief was to build a single-screen app with an appealing UI and simplicity at its core.

---

## 👤 Author

**Fabio Antonucci**
Apple Developer Academy 2025–26

---

## 📄 License

This project is available under the MIT License. See `LICENSE` for details.

---

<div align="center">

*your compass for your journey.*

🧭

</div>
