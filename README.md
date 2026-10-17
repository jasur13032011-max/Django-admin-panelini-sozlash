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
