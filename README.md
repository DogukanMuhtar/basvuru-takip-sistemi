# basvuru-takip-sistemi
Python ile geliştirilmiş dosya tabanlı başvuru takip sistemi
import json
import os
from datetime import date

DOSYA_ADI = "basvurular.json"

def verileri_yukle():
    if not os.path.exists(DOSYA_ADI):
        return []
    with open(DOSYA_ADI, "r", encoding="utf-8") as f:
        return json.load(f)

def verileri_kaydet(veriler):
    with open(DOSYA_ADI, "w", encoding="utf-8") as f:
        json.dump(veriler, f, indent=4, ensure_ascii=False)

def basvuru_ekle():
    veriler = verileri_yukle()

    basvuru = {
        "id": len(veriler) + 1,
        "ad_soyad": input("Ad Soyad: "),
        "tc_no": input("TC No: "),
        "basvuru_turu": input("Başvuru Türü: "),
        "tarih": str(date.today()),
        "durum": "Beklemede"
    }

    veriler.append(basvuru)
    verileri_kaydet(veriler)
    print(" Başvuru eklendi.")

def basvurulari_listele():
    veriler = verileri_yukle()
    if not veriler:
        print("Kayıt bulunamadı.")
        return

    for b in veriler:
        print(
            f"ID:{b['id']} | {b['ad_soyad']} | "
            f"{b['basvuru_turu']} | {b['durum']} | {b['tarih']}"
        )

def durum_guncelle():
    veriler = verileri_yukle()
    id = int(input("Başvuru ID: "))
    yeni_durum = input("Yeni Durum (Beklemede/Onaylandı/Reddedildi): ")

    for b in veriler:
        if b["id"] == id:
            b["durum"] = yeni_durum
            verileri_kaydet(veriler)
            print(" Durum güncellendi.")
            return

    print(" ID bulunamadı.")

def basvuru_sil():
    veriler = verileri_yukle()
    id = int(input("Silinecek Başvuru ID: "))

    yeni_liste = [b for b in veriler if b["id"] != id]

    if len(yeni_liste) == len(veriler):
        print(" ID bulunamadı.")
        return

    # ID'leri yeniden düzenle
    for i, b in enumerate(yeni_liste, start=1):
        b["id"] = i

    verileri_kaydet(yeni_liste)
    print(" Başvuru silindi.")

def menu():
    while True:
        print("\n--- BAŞVURU TAKİP SİSTEMİ ---")
        print("1 - Başvuru Ekle")
        print("2 - Başvuruları Listele")
        print("3 - Durum Güncelle")
        print("4 - Başvuru Sil")
        print("0 - Çıkış")

        secim = input("Seçiminiz: ")

        if secim == "1":
            basvuru_ekle()
        elif secim == "2":
            basvurulari_listele()
        elif secim == "3":
            durum_guncelle()
        elif secim == "4":
            basvuru_sil()
        elif secim == "0":
            print(" Çıkış yapıldı.")
            break
        else:
            print(" Geçersiz seçim.")

menu()
