# JonafanDwiyantoAlexander_Calypso
#include <iostream>
#include <string>
using namespace std;

// Fungsi cek vokal
bool isVowel(char c) {
    char lower = tolower(c);
    return (lower=='a'||lower=='e'||lower=='i'||lower=='o'||lower=='u');
}

// Fungsi reverse buatan sendiri
string customReverse(string s) {
    string rev = "";
    for (int i = s.length()-1; i >= 0; i--) {
        rev += s[i];
    }
    return rev;
}

// Fungsi pembuat sandi dari kata asli
string generatePassword(string word) {
    // 1. Hapus vokal
    string noVowel = "";
    for (char c : word) {
        if (!isVowel(c)) noVowel += c;
    }

    // 2. Balik urutan
    string reversed = customReverse(noVowel);

    // 3. Ambil ASCII huruf pertama
    int asciiCode = (int)word[0];
    string asciiStr = to_string(asciiCode);

    // 4. Sisipkan ASCII di tengah
    int mid = reversed.length()/2;
    string password = reversed.substr(0, mid) + asciiStr + reversed.substr(mid);

    return password;
}

// Fungsi rekonstruksi parsial dari sandi
string recoverPartialWord(string password) {
    // Cari posisi angka ASCII (2 digit, bisa diperluas)
    int pos = -1;
    for (int i = 0; i < password.length()-1; i++) {
        if (isdigit(password[i]) && isdigit(password[i+1])) {
            pos = i;
            break;
        }
    }
    if (pos == -1) return "Format sandi tidak valid.";

    // Ambil ASCII dan konversi ke huruf
    string asciiStr = password.substr(pos, 2);
    int asciiCode = stoi(asciiStr);
    char firstChar = (char)asciiCode;

    // Gabungkan huruf tanpa angka
    string combined = password.substr(0,pos) + password.substr(pos+2);
    string reversed = customReverse(combined);

    // Bentuk kerangka kata (tanpa vokal)
    string partial = firstChar + reversed.substr(1);

    return partial;
}

int main() {
    string word;
    cout << "Masukkan kata asli: ";
    cin >> word;

    // Pembentukan sandi
    string sandi = generatePassword(word);
    cout << "Sandi yang dihasilkan: " << sandi << endl;

    // Rekonstruksi parsial
    string hasil = recoverPartialWord(sandi);
    cout << "Hasil rekonstruksi parsial: " << hasil << endl;

    return 0;
}
