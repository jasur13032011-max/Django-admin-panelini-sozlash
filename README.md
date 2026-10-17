# Django-admin-panelini-sozlash
1. Superuser yaratish
Admin paneliga kirish uchun avval superuser (asosiy foydalanuvchi) yaratishingiz zarur. Terminal yoki buyruqlar satrida quyidagi buyruqni kiriting:

Bash
python manage.py createsuperuser
Shundan so'ng sizdan username (foydalanuvchi nomi), email va parol kiritish so'raladi.

2. Modelni admin.py da ro'yxatdan o'tkazish
Yaratgan modelingizni Django admin panelida ko'rsatish uchun uni ilovangizdagi admin.py faylida ro'yxatdan o'tkazasiz. Buning ikki xil usuli mavjud:

Oddiy usul:

Python
from django.contrib import admin
from .models import Post

admin.site.register(Post)
Dekorator yordamida (@admin.register):

Python
from django.contrib import admin
from .models import Post

@admin.register(Post)
class PostAdmin(admin.ModelAdmin):
    pass
3. list_display bilan ustunlarni belgilash
Admin panelidagi ro'yxat sahifasida model maydonlarini ustun ko'rinishida chiqarish uchun list_display atributidan foydalanasiz:

Python
from django.contrib import admin
from .models import Post

@admin.register(Post)
class PostAdmin(admin.ModelAdmin):
    list_display = ('id', 'title', 'author', 'created_at', 'is_active')
4. search_fields bilan qidiruv maydonini yoqish
Admin panelining yuqori qismida ma'lumotlarni qidirish maydonini qo'shish uchun search_fields dan foydalaniladi:

Python
from django.contrib import admin
from .models import Post

@admin.register(Post)
class PostAdmin(admin.ModelAdmin):
    list_display = ('id', 'title', 'author', 'created_at')
    search_fields = ('title', 'content') # Sarlavha va matn bo'yicha qidiradi
5. list_filter bilan o'ng tomondagi filtr panelini qo'shish
Ma'lumotlarni tezkor saralash va filtr qilish uchun o'ng panelga filtrlar qo'shish:

Python
from django.contrib import admin
from .models import Post

@admin.register(Post)
class PostAdmin(admin.ModelAdmin):
    list_display = ('id', 'title', 'author', 'created_at', 'is_active')
    search_fields = ('title', 'content')
    list_filter = ('is_active', 'created_at', 'author') # Filtrlar paneli
Hammasini jamlagan holda admin.py faylingiz quyidagi ko'rinishga keladi:

Python
from django.contrib import admin
from .models import Post

@admin.register(Post)
class PostAdmin(admin.ModelAdmin):
    list_display = ('id', 'title', 'author', 'created_at', 'is_active')
    search_fields = ('title', 'content')
    list_filter = ('is_active', 'created_at', 'author')
Shundan so'ng serverni ishga tushirib (python manage.py runserver), [http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/) manziliga kirib tekshirishingiz mumkin.

Tushunarli! Berilgan tanqidiy fikrlar va tavsiyalarga asosan, to'liq va amaliy ko'rinishdagi models.py hamda admin.py fayllarining kodlarini to'liq taqdim etaman.

Tasavvur qilaylik, bizda Blog ilovasi bor va unda Post modeli mavjud.

1. models.py (Modelni yaratish)
Ilovangizdagi models.py fayliga quyidagi kodni yozasiz:

Python
from django.db importवंशी import models # (yoki shunchaki from django.db import models)
from django.contrib.auth.models import User

class Post(models.Model):
    title = models.CharField(max_length=200, verbose_name="Sarlavha")
    content = models.TextField(verbose_name="Maqola matni")
    author = models.ForeignKey(User, on_delete=models.CASCADE, verbose_name="Muallif")
    created_at = models.DateTimeField(auto_now_add=True, verbose_name="Yaratilgan sana")
    is_active = models.BooleanField(default=True, verbose_name="Faolligi")

    def __str__(self):
        return self.title

    class Meta:
        verbose_name = "Maqola"
        verbose_name_plural = "Maqolalar"
2. admin.py (Admin panelda sozlash)
Endi admin.py faylida yuqoridagi modelni ro'yxatdan o'tkazib, ModelAdmin yordamida ustunlar, qidiruv va filtrlarni sozlaymiz:

Python
from django.contrib import admin
from .models import Post

@admin.register(Post)
class PostAdmin(admin.ModelAdmin):
    # Ro'yxat sahifasida ko'rinadigan ustunlar
    list_display = ('id', 'title', 'author', 'created_at', 'is_active')
    
    # Ustunlar bo'yicha havolalar (bosganda tahrirlash sahifasiga o'tadi)
    list_display_links = ('id', 'title')
    
    # Qidiruv panelida ishlatiladigan maydonlar
    search_fields = ('title', 'content')
    
    # O'ng tomondagi filtr paneli
    list_filter = ('is_active', 'created_at', 'author')
    
    # Ro'yxat sahifasida tahrirlash mumkin bo'lgan maydonlar
    list_editable = ('is_active',)
3. Ishga tushirish ketma-ketligi:
Agar buni o'z loyihangizda sinab ko'rmoqchi bo'lsangiz, terminalda quyidagi buyruqlarni ketma-ket bajarasiz:

Migratsiyalarni yaratish va bazaga qo'llash:

Bash
python manage.py makemigrations
python manage.py migrate
Superuser yaratish (agar yaratmagan bo'lsangiz):

Bash
python manage.py createsuperuser
Serverni yoqish:

Bash
python manage.py runserver
Shundan so'ng [http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/) manziliga kirib, Post modeli uchun barcha sozlangan ustunlar, filtrlar va qidiruv tizimi mukammal ishlayotganini ko'rishingiz mumkin!
