# Password-Cracking-Labs

Write-ups documenting my completion of the initial project modules from the **NetworkWalks Academy Cybersecurity & Ethical Hacking** course. All labs use a training file provided by the course (`My Locked PDF1.pdf` / `networkwalks_flag1.pdf`) — a password-protected PDF whose password is intentionally weak, for the purpose of learning password-auditing techniques.

> ⚠️ **Scope & ethics note:** These exercises were performed on a sample file supplied for training purposes by the course provider, in a lab/VM environment. Password cracking should only ever be performed on files/systems you own or are explicitly authorized to test.

---

## Background

Password cracking is the process of recovering a password from stored data or a protected file, used by security professionals to test password strength. Files such as PDF, ZIP, and Office documents store their password as a **hash** — a one-way scrambled representation. To recover the password you extract that hash from the file, then run it through a cracking tool that hashes candidate words/passwords and compares them against it (a **dictionary attack**).

* **Target file:** `My Locked PDF1.pdf` (a.k.a. `networkwalks_flag1.pdf`), 65.2 KB, PDF encryption revision 4 / V4, 128-bit key.

---

## Lab 1 — Password Cracking with John the Ripper (JTR) & Johnny

**Task:** Crack the password of `My Locked PDF1.pdf` using JTR John and JTR Johnny on Windows.

### Tools:
* [John the Ripper](https://openwall.com) (jumbo build, Windows binaries)
* [Johnny GUI](https://github.com) — GUI front-end for John the Ripper
* [OnlineHashCrack PDF Hash Extractor](https://onlinehashcrack.com) — pulls the crackable hash out of the PDF

### Steps:
1. Downloaded **John the Ripper** (jumbo, Windows x64) and installed the **Johnny GUI**, pointing Johnny's settings at `john.exe` inside the extracted `run` folder.
2. Uploaded `My Locked PDF1.pdf` to the **OnlineHashCrack PDF Hash Extractor** to convert the file's password protection into a crackable hash (`pdf2john` / `pdf2hashcat` format).
3. Copied the resulting hash — starting with `$pdf$*...` — into Notepad and saved it as `hash1.txt`.
4. In **Johnny**: Opened password file → selected `hash1.txt`. The hash loaded correctly, formatted as `PDF`.

<!-- 1. JOHNNY SETTINGS SCREENSHOT -->
![Johnny Tool Settings](images/1-johnny-settings.png)

5. Clicked **Start new attack** and let Johnny/John run its default cracking mode against the hash.
6. John recovered the password within the run; Johnny displayed it directly in the password column.
7. Opened `My Locked PDF1.pdf` in Adobe Acrobat Reader and entered the recovered password to confirm it unlocked the document.

### Result:
* **Status:** Password cracked successfully.
* **Recovered Password:** `password1`
* **Captured Flag:** `nw{cybersecurity_flag_captured_2608}`

<!-- 2. LAB 1 SUCCESS RESULT SCREENSHOT -->
![Lab 1 Success Result](images/2-lab1-success-flag.png)

### Learnings:
* `pdf2john` (or an equivalent online extractor) bridges PDF password protection into a format John understands.
* Johnny is just a GUI wrapper around the John the Ripper binary; all the cracking work happens in `john.exe`.
* A weak, dictionary-word password like `password1` is cracked almost instantly — reinforcing why longer, non-dictionary passwords matter.

---

## Lab 2 — Password Cracking with NetworkWalks Browser Tools

**Task:** Crack the password of `My Locked PDF1.pdf` using the NetworkWalks Hash Calculator and Password Cracker (both free, browser-based, no install required).

### Tools:
* [NetworkWalks Hash Calculator](https://networkwalks.com) — generates MD5/SHA family hashes and extracts a crackable hash from a password-protected PDF, all client-side in the browser.
* [NetworkWalks Password Cracker](https://networkwalks.com) — runs a dictionary attack against a pasted `$pdf$` hash, either with its built-in 100-word list or an uploaded wordlist.

### Steps:
1. Downloaded the encrypted PDF (`My Locked PDF1.pdf`) from the lab page.
2. Opened the **NetworkWalks Hash Calculator** and switched to the **PDF** tab.
3. Uploaded the locked PDF. The tool parsed it locally in the browser and reported it as encrypted, extracting a crackable hash.

<!-- 3. LAB 2 HASH CALCULATOR SCREENSHOT -->
![NetworkWalks Hash Calculator](images/3-hash-calculator.png)

4. Copied the full hash value.
5. Opened the **NetworkWalks Password Cracker**, pasted the hash into the **PDF HASH** field.
6. Left the built-in 100-password list active and clicked **Start Cracking**.
7. Watched the tool try candidate passwords live in the console output.
8. On try **#91** it matched: `[+] MATCH password1`. The tool displayed **"PASSWORD CRACKED SUCCESSFULLY — password1"**.

<!-- 4. LAB 2 CRACKING CONSOLE SCREENSHOT -->
![Browser Tool Cracking Console](images/4-password-cracker-success.png)

9. Opened `My Locked PDF1.pdf` and entered `password1` to unlock it, confirming the crack.

### Result:
* **Status:** Password cracked successfully (matched at 91/100 words tried, ~9 passwords/sec).
* **Recovered Password:** `password1`
* **Captured Flag:** `nw{networkwalks_flag_260821_1}`

<!-- 5. FINAL CONGRATULATIONS / FLAG CERTIFICATE SCREENSHOT -->
![Congratulations Flag Certificate](images/5-lab2-final-flag.png)

### Learnings:
* Both NetworkWalks tools run entirely client-side (**Web Crypto API** for hashing) — no file or text is uploaded to a server, per the tool's own disclosure.
* This mirrors the JTR workflow from Lab 1 conceptually (extract hash → dictionary attack) but packages it as a two-step, no-install browser experience for beginners.

---

## References

* [://networkwalks.com](https://://networkwalks.com)
