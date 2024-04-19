# Dokumentacja aplikacji „KostytuRAG”

## Opis uruchomienia Aplikacji

KostytuRAG został stworzony w pliku py w języku python z wykorzystaniem freameworka graficznego streamlit. Do uruchomiania aplikacji wymagane jest środowisko python(osobiście korzystałem z wersji pythona 3.12) oraz IDE obsługujące pliki pythonowe np. Visual Studio Code(z którego korzystałem). Do poprawnego działania aplikacji wymagane są następujące biblioteki:
langchain==0.1.16
langchain_community==0.0.33
langchain_core==0.1.43
langchain_openai==0.1.3
PyPDF2==3.0.1
python-dotenv==1.0.1
Requests==2.31.0
streamlit==1.33.0
faiss-cpu 
Spis potrzebnych bibliotek znajduję się również w pliku „requirements.txt”.
W pliku .env znajdują się klucze potrzebne do poprawnego działania aplikacji.
Klucz Open_Api_Key z pozyskany https://openai.com/.
 ![image](https://github.com/Mydlyk/KostytuRAG/assets/65900710/fd555376-d192-4d97-b510-683cefd2e92d)
  
Aplikację uruchomiamy poprzez wykonanie polecenia  w terminalu:
![image](https://github.com/Mydlyk/KostytuRAG/assets/65900710/b9b6fd1f-6b7f-4150-a44f-d467552d7cd6)
  
streamlit run KostytuRAG.py 
Uruchomiona aplikacja powinna znajdować na adresie http://localhost:8501/

##Opis uruchomienia Aplikacji Docker

Pierwszym krokiem jest zbudowanie obrazu z pliku Dockerfile. 
Przykładowa komenda budująca obraz:
 ![image](https://github.com/Mydlyk/KostytuRAG/assets/65900710/5f1d800c-050c-4a36-840a-9f89b974961c)

docker build -f Dockerfile -t kostyturag .  
Następnym krokiem jest uruchomienie/stworzenie kontenera z obrazu.
 Przykładowe polecenie:
 ![image](https://github.com/Mydlyk/KostytuRAG/assets/65900710/dfc59b00-39b2-4c80-b6bb-5900dc4d80ae)

docker run --name KostytuRAG -p 8501:8501 kostyturag 
Uruchomiona aplikacja powinna znajdować na adresie http://localhost:8501/.

##Opis działania aplikacji

Aplikacja jest chatem ze sztuczną inteligencją ,który odpowiada na pytania wyłącznie dotyczące polskiej konstytucji.
Aplikacja pobiera polską konstytucję z Internetu oraz konwertuję ją i zapisuje w bazie wektorowej FAISS. Na podstawie tego dokumentu odpowiada tylko na pytania powiązane z polską konstytucją. Na inne pytania odpowiada „Tego nie zawarto w polskiej konstytucji”. LLM openAi odpowiada w języku polskim i utrzymuje kontekst wypowiedzi.
## Opis kodu aplikacji

![image](https://github.com/Mydlyk/KostytuRAG/assets/65900710/f0e24927-7ddf-46fa-9c0c-9fa44d161831)

 
Funkcja ta odpowiada za pobrania polskiej konstytucji z url.
 ![image](https://github.com/Mydlyk/KostytuRAG/assets/65900710/31c4ab55-eb07-4ec9-8ef2-efb25d1c8107)

Polska konstytucja jest pobierana w formacie pdf więc funkcja get_text_from_pdf przerabia pdf na text.
 ![image](https://github.com/Mydlyk/KostytuRAG/assets/65900710/53b7cd24-37d1-4bda-96df-34ff21c6bdb7)

Powyższa funkcja zapisuje Konstytucje w bazie wektorowej.
![image](https://github.com/Mydlyk/KostytuRAG/assets/65900710/c18bc8b5-2e2b-4fe5-92a9-b42605b0d90e)
 
Powyższa funkcja tworzy łańcuch przetwarzania dla sztucznej inteligencji z główną zasadą oraz polską konstytucją.
![image](https://github.com/Mydlyk/KostytuRAG/assets/65900710/b929fd6d-869d-47e0-8067-de92b596f692)
 
Powyższa funkcja tworzy łańcuch dla łańcucha powiązania z pytaniem od użytkownika.
W zmiennej llm jest możliwe dokładne ustawienie modelu i temperatury api.
![image](https://github.com/Mydlyk/KostytuRAG/assets/65900710/69f8fd48-d1a2-42eb-9a4e-fe6d97c71d5b)
 
Funkcja get_response zwraca odpowiedz sztucznej inteligencji na pytanie na podstawie polskiej konstytucji.
 ![image](https://github.com/Mydlyk/KostytuRAG/assets/65900710/211f71d0-3c1b-4047-a146-71308c762bcd)

Jeśli baza wektorowa jest pusta następuje pobranie konstytucji i jej zapisanie.
Dodanie do historii chatu nowych odpowiedzi oraz zaznaczenie przez kogo została ona dodana.
![image](https://github.com/Mydlyk/KostytuRAG/assets/65900710/e464b078-51af-42ed-86e2-33d2e3e485da)
![image](https://github.com/Mydlyk/KostytuRAG/assets/65900710/efceab58-0556-4589-821c-a9d8e82d0a9b)
 
 
Dodanie do historii chatu nowych odpowiedzi oraz zaznaczenie przez kogo została ona dodana.
![image](https://github.com/Mydlyk/KostytuRAG/assets/65900710/64c5af21-b232-439c-b86c-e4e53cddefe9)

 
Ten kod odpowiada za wyświetlanie chatu.
 ![image](https://github.com/Mydlyk/KostytuRAG/assets/65900710/b5f86691-c928-4ad9-8a07-b28029b28ec7)

Ten kod javascriptowy odpowiada za automatyczne scrolowanie w dół strony po dodaniu nowej wiadomości na wzór działania chatu gpt.

## Działanie aplikacji
 ![image](https://github.com/Mydlyk/KostytuRAG/assets/65900710/5e2d3e75-d551-472b-9a68-15e1abe6b65d)

Rysunek 1 Początek aplikacji
 ![image](https://github.com/Mydlyk/KostytuRAG/assets/65900710/eff4fa32-aa13-44ce-9196-697aeea2d6c2)

Rysunek 2 Działanie aplikacji
![image](https://github.com/Mydlyk/KostytuRAG/assets/65900710/a4806a14-512e-4e0e-8345-2d07c3443f81)

Rysunek 3 Sprawdzenie czy chat odpowie na pytanie nie powiązane z polską konstytucją
![image](https://github.com/Mydlyk/KostytuRAG/assets/65900710/aec7a300-29ca-4d66-a53c-7f8686baa4d4)
 
Rysunek 4 Utrzymywanie kontekstu wypowiedzi

