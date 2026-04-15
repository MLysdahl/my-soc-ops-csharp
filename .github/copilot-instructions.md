# Copilot Workspace Instructions

## 🔶 MANDATORY CHECKS (Before Commit)

- [ ] \dotnet build SocOps/SocOps.csproj\ - Zero errors
- [ ] \dotnet format --verify-no-changes\ - Code formatted
- [ ] \dotnet test\ - All tests pass

---

## Project

**Soc Ops** — Social Bingo game (Blazor WebAssembly, .NET 10) for meetings/conferences. Find people matching questions, get 5 in a row to win.

**Stack**: Blazor WASM, C# 13, Bootstrap, custom CSS utilities, event-driven state management

## Architecture

\\\
SocOps/
├── Components/      # BingoBoard, BingoSquare, GameScreen, StartScreen
├── Models/          # GameState, BingoSquareData, BingoLine
├── Services/        # BingoGameService (state), BingoLogicService (logic)
├── Data/            # Questions.cs (bingo content)
├── Layout/          # MainLayout, NavMenu
├── Pages/           # Home, Counter, NotFound
└── wwwroot/         # index.html, css/app.css, bootstrap
\\\

## Essentials

**Commands**:
\\\ash
dotnet build SocOps/SocOps.csproj        # Build
dotnet run --project SocOps             # Run (localhost:5166)
dotnet format SocOps/SocOps.csproj      # Format code
\\\

**Conventions**:
- PascalCase (public), camelCase (private)
- Components: minimal logic → services
- Services: event-driven pattern with \OnStateChanged\
- CSS: use utility classes (.flex, .grid, .p-4, .bg-primary, etc.)

**State Management**:
- \BingoGameService\ = single source of truth
- Components subscribe to \OnStateChanged\ event
- Persists to localStorage via JSInterop

**Component Pattern**:
\\\csharp
@implements IAsyncDisposable

protected override async Task OnInitializedAsync()
{
    _gameService.OnStateChanged += OnGameStateChanged;
}

async ValueTask IAsyncDisposable.DisposeAsync()
{
    _gameService.OnStateChanged -= OnGameStateChanged;
}
\\\

## Key Paths

- Game logic: \Services/BingoGameService.cs\
- UI Components: \Components/*.razor\
- Styles: \wwwroot/css/app.css\
- Questions: \Data/Questions.cs\
- Design guide: \.github/instructions/frontend-design.instructions.md\
- CSS utils: \.github/instructions/css-utilities.instructions.md\

## Tips

1. \BingoGameService\ = authoritative state source
2. Check existing components before creating new ones
3. Use utility classes; avoid inline CSS
4. Keep components lightweight
5. Design directions: see frontend-design.instructions.md for anti-patterns to avoid

---

*Last Updated: April 2026*
