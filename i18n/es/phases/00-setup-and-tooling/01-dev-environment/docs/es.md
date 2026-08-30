# Medio ambiente de desarrollo

> Sus herramientas dan forma a su pensamiento.

**Type:** Build
**Languages:** Python, Node.js, Rust
**Prerequisites:** None
**Time:** ~45 minutes

## Objetivos de aprendizaje

- Configure Python 3.11+, Node.js 20+, y cadenas de herramientas Rust desde cero
- Configurar entornos virtuales y administradores de paquetes para edificaciones reproducibles
- Verificar el acceso de la GPU con CUDA/MPS y ejecutar una operación de tensor de prueba
- Comprender la pila de cuatro capas: sistema, paquetes, tiempos de ejecución, bibliotecas de IA

## El problema

Estás a punto de aprender ingeniería de IA en más de 500 lecciones usando Python, TypeScript, Rust y Julia. Si tu entorno se rompe, cada lección se convierte en una lucha contra la herramienta en lugar de aprender.

La mayoría de la gente omite la configuración del entorno y luego pasa horas debujando errores de importación, conflictos de versiones y controladores de CUDA faltantes.

## El concepto

Un entorno de ingeniería de IA tiene cuatro capas:

```mermaid
graph TD
    A["4. AI/ML Libraries\nPyTorch, JAX, transformers, etc."] --> B["3. Language Runtimes\nPython 3.11+, Node 20+, Rust, Julia"]
    B --> C["2. Package Managers\nuv, pnpm, cargo, juliaup"]
    C --> D["1. System Foundation\nOS, shell, git, editor, GPU drivers"]
```

Installamos abajo arriba. Cada capa depende de la que está debajo de ella.

```figure
s0-env-stack
```

## Construye el mismo

### Paso 1: Fundamento del sistema

Compruebe su sistema e instale los elementos básicos.

```bash
# macOS
xcode-select --install
brew install git curl wget

# Ubuntu/Debian
sudo apt update && sudo apt install -y build-essential git curl wget

# Windows (use WSL2)
wsl --install -d Ubuntu-24.04
```
wsl --install -d Ubuntu-24.04 esto podría dar error como la versión específica mencionada es el problema simplemente poner 
wsl --instalar -d Ubuntu

### Paso 2: Python con UV

Usamos`uv`Es 10-100 veces más rápido que pip y maneja entornos virtuales automáticamente.

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh      #if using windows remember to use bash instead of powershell

uv python install 3.12

uv venv
source .venv/bin/activate  # or .venv\Scripts\activate on Windows
#I used  source .venv/Scripts/activate  to activate the virtual environment in bash in windows 

uv pip install numpy matplotlib jupyter
```

Verifique:

Para verificar que Python ya estaba instalado usando el gerente de paquetes uv simplemente escriba Python en bash se abrirá Python interpreter

```python
import sys
print(f"Python {sys.version}")

import numpy as np
print(f"NumPy {np.__version__}")
a = np.array([1, 2, 3])
print(f"Vector: {a}, dot product with itself: {np.dot(a, a)}")
```

### Paso 3: Node.js con pnpm

Para clases de TypeScript (agentes, servidores MCP, aplicaciones web).

```bash 
fnm stands for Fast Node Manager. It's a Node.js version manager written in Rust that lets you:

What it does:

Install and switch between multiple versions of Node.js
Manage Node.js versions per project or globally
Automatically use the right Node.js version for each project

```

```bash
You can't install fnm with uv because fnm is a Node.js version manager (not a Python package), while uv is for Python package

```

```bash
curl -fsSL https://fnm.vercel.app/install | bash
# for me above command curl didn't worked use in powershell winget install Schniz.fnm
# restart the terminal or open a new terminal cmd  whatever
fnm install 22
fnm use 22

npm install -g pnpm

node -e "console.log('Node', process.version)"
```

como lo instale usando winget instalar schniz.fnm 
```bash

Start with step 3 if this command does not work notepad $PROFILE then follow 
No problem — your PowerShell profile folder/file doesn't exist yet. Let's create it.

Run these one by one in PowerShell:

Step 1
 New-Item -ItemType Directory -Force -Path (Split-Path $PROFILE)


Then:
Step 2 
New-Item -ItemType File -Force -Path $PROFILE


Now open it:
Step 3
notepad $PROFILE


Paste this line into Notepad:
Step 4
fnm env --use-on-cd | Out-String | Invoke-Expression


Save and close Notepad.

Then close the PowerShell/VS Code terminal and open a new one.

Test:

Step 5
fnm --version


Then:

fnm install 22

// below command was not working  current PowerShell session still isn't loading the FNM environment.

Troubleshoot
Perfect 👍 FNM is working now.

That warning just means Node.js 22.23.2 is already installed, so you don't need to install it again.

Run:

fnm use 22


Then:

node -v


You should see:

v22.23.2


And check npm:

npm -v

If fnm use 22 works

You're all set. 🎉

For your AI Engineering From Scratch project, you can now run:

npm init -y


or, if you're starting a project with a framework, we can set that up next.

still if fnm use 22 doesn't work then

Got it — FNM itself is installed, but your current PowerShell session still isn't loading the FNM environment.

Let's fix it directly.

1. Check your profile path

Run:

echo $PROFILE


It should show something like:

C:\Users\deepa\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1

2. Add FNM to the profile automatically

Run this exact command:

"fnm env --use-on-cd | Out-String | Invoke-Expression" | Add-Content -Path $PROFILE

3. Load the profile immediately

Instead of restarting, run:

. $PROFILE


Then check:

fnm env


You should see several environment-variable commands.

4. Now use Node 22
fnm use 22


Then:

node -v


Expected:

v22.23.2

If you still get the same error

Run these two commands and send me the output:

echo $PROFILE


and

Get-Content $PROFILE


I'll tell you exactly what needs to be changed.
fnm use 22
node -v


You should get a Node version beginning with v22.


still use not working was getting 3 lines

Great — your profile is correct and FNM is configured. You just have the same line 3 times, which isn't necessary.

Let's clean it up and make sure it loads correctly.

1. Replace your profile with one FNM line

Run:

Set-Content $PROFILE 'fnm env --use-on-cd | Out-String | Invoke-Expression'


Now verify:

Get-Content $PROFILE


You should see only one line:

fnm env --use-on-cd | Out-String | Invoke-Expression

2. Reload the profile

Run:

. $PROFILE

3. Check FNM

Run:

fnm --version


You should get:

1.39.0

4. Use Node 22

Now run:

fnm use 22


Then:

node -v


You should get:

v22.23.2

If it STILL gives the environment-variable error

Run:

fnm env


and send me the output. Don't reinstall Node or FNM yet — your installation appears fine; we're just fixing the PowerShell environment.

then if this is the error shown in powershell


PS C:\Users\deepa\OneDrive\Desktop\AI Engineering From Scratch> . $PROFILE
. : File C:\Users\deepa\OneDrive\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1 cannot be loaded because
running scripts is disabled on this system. For more information, see about_Execution_Policies at
https:/go.microsoft.com/fwlink/?LinkID=135170.
At line:1 char:3
+ . $PROFILE
+   ~~~~~~~~
    + CategoryInfo          : SecurityError: (:) [], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess

then

Aha — now we found the actual problem. 👍

FNM is installed correctly. Your PowerShell execution policy is blocking your profile script from running.

Let's fix that.

1. Allow your PowerShell profile to run

Run this in PowerShell:

Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser


If PowerShell asks:

Execution Policy Change
[Y] Yes  [A] Yes to All  [N] No ...


Type:

Y


and press Enter.

2. Load your profile

Now run:

. $PROFILE


There should be no error.

3. Use Node 22
fnm use 22


Then:

node -v


You should get:

v22.23.2


And:

npm -v

4. Verify everything

You can run:

fnm current
node -v
npm -v


If node -v shows v22.23.2, you're completely set. 🚀

Don't reinstall FNM or Node. The issue was simply PowerShell's script execution policy.




```


**macOS / Apple Silicon (M1/M2/M3/M4):**Si el instalador deja de instalar `Error: Cannot install under Rosetta 2 in ARM default prefix (/opt/homebrew)`, su terminal está funcionando bajo Rosetta 2 (`arch`huellas`i386`Instálle el arm64 forzador de fnm, cablealo en su caparazón, y luego vuelva a ejecutar los comandos anteriores desde `fnm install 22`¿Qué es esto ?

```bash
arch -arm64 brew install fnm
echo 'eval "$(fnm env --use-on-cd)"' >> ~/.zshrc
source ~/.zshrc
```

### Paso 4: Corrosidad

Para las lecciones críticas al rendimiento (inferencia, sistemas).

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

rustc --version
cargo --version
```

### Paso 5: Julia (opcional)

Para clases de matemáticas pesadas donde Julia brilla.

```bash
curl -fsSL https://install.julialang.org | sh

julia -e 'println("Julia ", VERSION)'
```

### Paso 6: Configuración de GPU (si tiene uno)

**NVIDIA (Linux / Windows):**

```bash
nvidia-smi

# Install PyTorch with CUDA
uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124
```

**macOS / Apple Silicon (M1/M2/M3/M4):**No hay CUDA en un Mac que se espera, no un fracaso.**not**Pasé .`--index-url .../cuXXX`(aquellas ruedas son solo Linux / Windows, por lo que la instalación falla). Instale la construcción simple, que incluye el backend de la GPU MPS (Metal) de Apple:

```bash
uv pip install torch torchvision torchaudio
```

Verificar (funciona en cualquier plataforma):

```python
import torch
print(f"CUDA available: {torch.cuda.is_available()}")           # False on macOS — expected
print(f"MPS available:  {torch.backends.mps.is_available()}")   # True on Apple Silicon
if torch.cuda.is_available():
    print(f"GPU: {torch.cuda.get_device_name(0)}")
```

No hay GPU? No hay problema. La mayoría de las clases funcionan en CPU. Para las clases pesadas de entrenamiento, utilice Google Colab o GPUs en la nube.

### Paso 7: Verifique la ruta que desea iniciar

ejecuta cada comando en esta lección desde la raíz del repositorio, el directorio que
contiene `README.md`y `phases/`El prevuelo sólo comprueba lo que necesitas .
comienza la ruta seleccionada. Salta las herramientas posteriores por defecto para que un nuevo aprendiz vea
una respuesta clara en lugar de un muro de advertencias.

Comience la secuencia completa de principiantes:

```bash
python3 phases/00-setup-and-tooling/01-dev-environment/code/verify.py --route beginner
```

O sólo compruebe la ruta que quieras:

```bash
python3 phases/00-setup-and-tooling/01-dev-environment/code/verify.py --route ml-foundations
python3 phases/00-setup-and-tooling/01-dev-environment/code/verify.py --route llm-engineering
python3 phases/00-setup-and-tooling/01-dev-environment/code/verify.py --route agents
python3 phases/00-setup-and-tooling/01-dev-environment/code/verify.py --route mcp
python3 phases/00-setup-and-tooling/01-dev-environment/code/verify.py --route agent-skills
python3 phases/00-setup-and-tooling/01-dev-environment/code/verify.py --route certification
```

Añadir`--show-later`cuando se desea el mismo prevuelo para inspeccionar herramientas opcionales
Una herramienta posterior que no haya sido utilizada nunca bloqueará la
la ruta seleccionada.

Cada verificación requerida fallida incluye el camino detectado o el error de importación y un
Las habilidades de los agentes y las rutas de certificación también muestran
las comprobaciones manuales del host porque un script Python no puede probar que un host de IA tiene
Descubre una habilidad o que el alcance de habilidad que elijas es escritorio.

Cuando el prevuelo de principiante pasa, imprime la primera lección ejecutable exacta:

```text
Ready to start Beginner course.
Next: python3 phases/01-math-foundations/01-linear-algebra-intuition/code/vectors.py
```

## Usalo

Su entorno está listo para comenzar la ruta que comprobó.
Cuando una lección pide por ellos en lugar de bloquear su primera lección en su conjunto
Esto es lo que usará en todo el plan de estudios:

| Language | Used In | Package Manager |
|----------|---------|-----------------|
| Python | Phases 1-12 (ML, DL, NLP, Vision, Audio, LLMs) | uv |
| TypeScript | Phases 13-17 (Tools, Agents, Swarms, Infra) | pnpm |
| Rust | Phases 12, 15-17 (Performance-critical systems) | cargo |
| Julia | Phase 1 (Math foundations) | Pkg |

## Envío

Esta lección produce un guión de verificación que cualquiera puede ejecutar para comprobar su configuración.

¿ Qué ?`outputs/prompt-env-check.md`para una respuesta que ayuda a los asistentes de IA a diagnosticar problemas ambientales.

## Los ejercicios

1. Ejecutar el guión de verificación y corregir cualquier falla
2. Crear un entorno virtual Python para este curso e instalar PyTorch
3. Escriba un "hola mundo" en los cuatro idiomas y ejecuta cada uno
