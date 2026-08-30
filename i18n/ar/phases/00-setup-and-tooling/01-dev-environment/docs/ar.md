# بيئة التنمية

> أدواتك تشكل تفكيرك، قم بتنظيمها مرة واحدة، قم بتنظيمها بشكل صحيح.

**Type:** Build
**Languages:** Python, Node.js, Rust
**Prerequisites:** None
**Time:** ~45 minutes

## أهداف التعلم

- قم بتعيين Python 3.11+، Node.js 20+، و Rust من الصفر
- تكوين بيئات افتراضية ومديري الحزم للمبنيات المتكملة
- التحقق من وصول GPU مع CUDA / MPS وإجراء عملية اختبار التنسور
- فهم كومة أربع طبقات: النظام، الحزم، أوقات تشغيل، مكتبات الذكاء الاصطناعي

## المشكلة

أنت على وشك تعلم هندسة الذكاء الاصطناعي عبر 500 دروس تستخدم بايثون، الكتابة النمطية، الخرق، وجوليا. إذا تم تدمير بيئتك، كل دروس واحد يصبح معركة ضد الأدوات بدلا من التعلم.

معظم الناس يخطون إعداد البيئة ثم يقضون ساعات في تحليل أخطاء الاستيراد، نزاعات الإصدارات، ووقودات CUDA المفقودة. سنقوم بهذا مرة واحدة، بشكل صحيح.

## المفهوم

بيئة هندسة الذكاء الاصطناعي لديها أربع طبقات:

```mermaid
graph TD
    A["4. AI/ML Libraries\nPyTorch, JAX, transformers, etc."] --> B["3. Language Runtimes\nPython 3.11+, Node 20+, Rust, Julia"]
    B --> C["2. Package Managers\nuv, pnpm, cargo, juliaup"]
    C --> D["1. System Foundation\nOS, shell, git, editor, GPU drivers"]
```

نضع من أسفل إلى أعلى كل طبقة تعتمد على الطبقة التي تحتها

```figure
s0-env-stack
```

## بناءها

### الخطوة الأولى: أساس النظام

تحقق من نظامك ووضع الأساسيات

```bash
# macOS
xcode-select --install
brew install git curl wget

# Ubuntu/Debian
sudo apt update && sudo apt install -y build-essential git curl wget

# Windows (use WSL2)
wsl --install -d Ubuntu-24.04
```
wsl --install -d Ubuntu-24.04 هذا قد يعطي خطأ كما النسخة المحددة المذكورة هي المشكلة ببساطة وضعت 
wsl --install --d Ubuntu

### الخطوة الثانية: Python مع UV

نحن نستخدم`uv` هو 10-100x أسرع من البوابة ويعالج البيئات الافتراضية تلقائيا.

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh      #if using windows remember to use bash instead of powershell

uv python install 3.12

uv venv
source .venv/bin/activate  # or .venv\Scripts\activate on Windows
#I used  source .venv/Scripts/activate  to activate the virtual environment in bash in windows 

uv pip install numpy matplotlib jupyter
```

التحقق من:

للتحقق من أن Python قد تم تثبيته بالفعل باستخدام مدير حزم uv ببساطة كتابة Python في bash فسوف تفتح Python مترجم

```python
import sys
print(f"Python {sys.version}")

import numpy as np
print(f"NumPy {np.__version__}")
a = np.array([1, 2, 3])
print(f"Vector: {a}, dot product with itself: {np.dot(a, a)}")
```

### الخطوة 3: Node.js مع pnpm

للدرسات TypScript (وكلاء، خادمات MCP، تطبيقات الويب).

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

كما قمت بتثبيتها باستخدام winget تنسيق schniz.fnm 
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


**macOS / Apple Silicon (M1/M2/M3/M4):**إذا توقف التركيب عن`Error: Cannot install under Rosetta 2 in ARM default prefix (/opt/homebrew)`، محطةك تعمل تحت " روزيتا 2 "`arch`بصمات`i386`(هومبريو) هو بناء أليف أرم64، قم بتثبيت (أرم64) القسري، قم بتسجيله في قذفك، ثم قم بإعادة تشغيل الأوامر أعلاه من (أرم64).`fnm install 22`:

```bash
arch -arm64 brew install fnm
echo 'eval "$(fnm env --use-on-cd)"' >> ~/.zshrc
source ~/.zshrc
```

### الخطوة الرابعة: الدرونة

للدروس الحرجة للأداء (الاضافة، الأنظمة).

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

rustc --version
cargo --version
```

### الخطوة 5: جوليا (اختياري)

لدروس الرياضيات الثقيلة حيث يضيء جوليا.

```bash
curl -fsSL https://install.julialang.org | sh  #for me used windows package manager winget install julia -s msstore

julia -e 'println("Julia ", VERSION)'
```

### الخطوة 6: إعداد GPU (إذا كان لديك واحد)

**NVIDIA (Linux / Windows):**

```bash
nvidia-smi

# Install PyTorch with CUDA
uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124
```

**macOS / Apple Silicon (M1/M2/M3/M4):**لا يوجد أي "كودا" على "ماك" متوقعة، لا فشل.**not**مرر`--index-url .../cuXXX`(هذه العجلات هي لينكس ويندوز فقط، لذلك فشل التثبيت). قم بتثبيت البناء العادي، الذي يتضمن MPS (المعدل) GPU الخلفي من آبل:

```bash
uv pip install torch torchvision torchaudio
```

التحقق (يعمل على أي منصة):

```python
import torch
print(f"CUDA available: {torch.cuda.is_available()}")           # False on macOS — expected
print(f"MPS available:  {torch.backends.mps.is_available()}")   # True on Apple Silicon
if torch.cuda.is_available():
    print(f"GPU: {torch.cuda.get_device_name(0)}")
```

لا يوجد نظام معين للكمبيوترات (GPU) ؟ لا مشكلة. معظم الدروس تعمل على جهاز معين للكمبيوترات (CPU).

### الخطوة 7: تحقق من المسار الذي تريد البدء به

إشغال كل أمر في هذا الدروس من جذور مخزن، المجلد الذي
يحتوي على`README.md`و`phases/`قبل الرحلة يُفحص فقط ما تحتاج إليه
يبدأ الطريق المحدد. يفرض أدوات لاحقة بشكل افتراضي حتى يرى المتعلم الجديد
رد واضح بدلاً من جدار من التحذيرات

ابدأ سلسلة المبتدئين الكاملة:

```bash
python3 phases/00-setup-and-tooling/01-dev-environment/code/verify.py --route beginner
```

أو تحقق فقط من الطريق الذي تريده:

```bash
python3 phases/00-setup-and-tooling/01-dev-environment/code/verify.py --route ml-foundations
python3 phases/00-setup-and-tooling/01-dev-environment/code/verify.py --route llm-engineering
python3 phases/00-setup-and-tooling/01-dev-environment/code/verify.py --route agents
python3 phases/00-setup-and-tooling/01-dev-environment/code/verify.py --route mcp
python3 phases/00-setup-and-tooling/01-dev-environment/code/verify.py --route agent-skills
python3 phases/00-setup-and-tooling/01-dev-environment/code/verify.py --route certification
```

إضافة`--show-later`عندما تريد نفس التحقيق قبل الرحلة للتفتيش الأدوات الاختيارية
و الاعتماد المستخدمة في الدروس اللاحقة.
الطريق المختار.

كل عملية فاشلة مطلوبة تتضمن المسار المكتشف أو خطأ الاستيراد
القيادة التصحيحية الدقيقة. مهارات العميل وطرق الشهادة أيضاً
التحقق من المضيف اليدوي لأن نص Python لا يمكن أن يثبت أن المضيف AI
اكتشفت مهارة أو أن المهارات التي اخترتها قابلة للكتابة

عندما يمر المبتدئ قبل الرحلة، فإنه يطبخ الدروس الأولى الدراسة:

```text
Ready to start Beginner course.
Next: python3 phases/01-math-foundations/01-linear-algebra-intuition/code/vectors.py
```

## استخدمها

بيئتك جاهزة لبدء المسار الذي تحققت منه
عندما يطلب الدروس منهم بدلاً من حظر دروسك الأولى على الإطلاق
هنا ما ستستخدمه في جميع أنحاء المناهج الدراسية:

| Language | Used In | Package Manager |
|----------|---------|-----------------|
| Python | Phases 1-12 (ML, DL, NLP, Vision, Audio, LLMs) | uv |
| TypeScript | Phases 13-17 (Tools, Agents, Swarms, Infra) | pnpm |
| Rust | Phases 12, 15-17 (Performance-critical systems) | cargo |
| Julia | Phase 1 (Math foundations) | Pkg |

## أرسله

هذا الدروس ينتج نص التحقق يمكن لأي شخص تشغيله للتحقق من إعداداتهم.

انظر`outputs/prompt-env-check.md`لمستشارات تساعد مساعدات الذكاء الاصطناعي على تشخيص مشاكل البيئة

## التمارين

1. إشغال النص التحقق وإصلاح أي أخطاء
2. قم بإنشاء بيئة افتراضية Python لهذا الدورة و قم بتثبيت PyTorch
3. اكتب "مرحباً للعالم" بكلّ الألغات الأربعة و إشغلي كلّها
