# 🌱 Multilingual Seed Phrase Compatibility — Thoughts on BIP39

If you have been into the bitcoin space — or really even if you haven’t — you may have encountered something that looks close to this, for eg:

```green toy final snack divorce police ...```

These innocuous looking words together is called a **Seed Phrase**. This is the phrase that is responsible for helping you back up your wallet if things go south. You can consider them as your Bitcoin version of password, in general terms.

---

## The Problem

The problem with this is…these are in **English**.

What if, I as an non-native English speaker (although fluent in English), want to use a wallet in my local language? You would assume that there exists some sort of system for it to be made compatible to other languages?

**The answer: Yesn’t.**

You see, this was the original idea of the person(s) who proposed the **BIP39 Standard** (BIP stands for the Bitcoin Improvement Proposal). They wanted a standard way to generate a Seed from an easy to understand mnemonic consisting of 2048 words. The algorithm essentially encodes the randomness that is due to choosing any (valid) set of 12/15/24 words from the 2048.

It is important to note that the generation of **entropy** is independent of the words of the Seed Phrase. All that is needed for that is something that encodes randomness. It could be seed phrase words, and in the case of **SeedSigner** (a popular open source hardware wallet): it can even be **coinflips and dice rolls**!

---

## 🔐 Where the Problem Gets Real: Key Derivation

The problem — as mentioned in the title — arises in the generation of keys from this entropy. Key derivation functions need to be computationally intensive in order to be secure, that hashing as much stuff that should be available to the user as possible (at least I think that is what the idea is).

The **Key Derivation Function** ([explained very well here](https://learnmeabitcoin.com/technical/keys/hd-wallets/mnemonic-seed/#generate-entropy)) that Bitcoin uses is based on hashing together the **Seed Phrase words**, which as you can imagine is very computationally intensive. 

But note the key thing here:  the key derivation function depends upon the **seed phrase words**,  **while the entropy doesn’t!**

---

This is in my opinion a major flaw in the design of BIP39. If you happen to be using a Spanish wordlist for example, and you switch to an incompatible wallet only using English wordlists — you cannot back up the wallet.

Despite both of them having the **same entropy on paper**, it is a big issue.

---

What I present (and I am not unique in this, and it's also not the most ingenious solution ever) is a way to **convert each word in a non-English wordlist to an English wordlist (standardized)** so that non-English wordlists can be easily used to encode entropy, and the English counterparts of them can be used in the **PBKDF2 function** to create the keys.

This way even if you are switching to a non-compatible wallet, you just need — for each word — its corresponding English word to make it compatible.

---

## 🔁 Example Mapping
```
ábaco -> abandon
abierto -> about
...
...
```

In the image below, just as **coinflips are converted into seed phrase words in English (Mnemonic sentences)**, a similar method can be used for converting (for eg.) Spanish wordlist words to English.
![image](https://github.com/user-attachments/assets/d3bc8c81-8081-47b0-80b7-95589753061b)

I mean, if we can generate private keys from coinflips, what’s stopping us from supporting other wordlists?

Just something I’ve been thinking for quite some time…

