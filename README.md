#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <stdbool.h>
#include <windows.h>
#include <math.h>

// Структура акаунта
typedef struct Account {
    char name[50];
    char email[50];
    char password[50];
} Account_information;

// Структура зі списком музики
typedef struct Music {
    const char *musicname;
    int time;
} Music_list;

// Список музики
Music_list songs[] = {
    { "Kalush - Stefania", 179 },
    { "Kalush - Додому", 216 },
    { "Kalush - Файна", 156 },
    { "Kalush feat. Tember Blanche - Калуські вечорниці", 180 },
    { "Kalush - Зорі", 200 },
    { "YAKTAK feat. Sobol' - Погляд", 177 },
    { "Антитіла - Фортеця Бахмут", 199 },
    { "SadSvit, СТРУКТУРА ЩАСТЯ - Силуети", 173 },
    { "Без обмежень - Без неї ніяк", 282 },
    { "alyona alyona - Не втратимо зв'язок", 149 },
    { "SKOFKA - Чути гімн", 148 }
};

int size = sizeof(songs) / sizeof(songs[0]); // Глобальне оголошення змінної size

Account_information *myAccount; // Оголошення глобального вказівника на структуру Account_information

void Menu(); // Прототип функції меню програми

void Account_registration(); // Прототип функції створення акаунта

void Output_of_account_data(); // Прототип функції виведення даних про акаунт

void List_of_music(); // Прототип функції, яка виводить список музики користувачеві

void Music_search(); // Прототип функції, яка веде пошук музики

void Account_deletion(); // Прототип функції видалення акаунта

int main()
{
    SetConsoleCP(1251);
    SetConsoleOutputCP(1251);

    Account_registration();
    Menu();

    return 0;
}

void Menu()
{
    int choice = -1;

    if (myAccount!=0){
    do {
        printf("\nМеню:\n");
        printf("1) Список доступних пісень\n");
        printf("2) Пошук музики за назвою\n");
        printf("3) Виведення даних про акаунт\n");
        printf("4) Видалення акаунта\n");
        printf("5) Зміна даних акаунта\n");
        printf("6) Вийти з програми\n");

        bool check = true;

        do {
            printf("\nВиберіть опцію: ");

            // Перевірка на коректне введення
            if (scanf("%d", &choice) != 1) {
                printf("\nПомилкове введення. Спробуйте знову\n");

                // Очищення буферу вводу
                while (getchar() != '\n');

                // Продовження циклу з нового кроку
                continue;
            } else {
                check = false;
            }

        } while (check);

        switch (choice) {
            case 1:
                List_of_music();
                break;
            case 2:
                Music_search();
                break;
            case 3:
                Output_of_account_data();
                break;
            case 4:
                Account_deletion();
                break;
            case 5:
                changeAccount();
                break;
            case 6:
                printf("\nПрограма завершена\n");
                break;
            default:
                printf("Невірний вибір. Спробуйте ще раз\n");
                break;
        }
    } while (choice != 6);
}
else{
    Account_registration();
}
}

void Account_registration() // Оголошення функції регістрації нового акаунта
{
    myAccount = (struct Account*)malloc(sizeof(struct Account)); // Виділення пам'яті під структуру

    // Перевірка, чи була виділена пам'ять
    if (myAccount == NULL) {
        printf("Помилка виділення пам'яті для myAccount\n");
        return;
    }

    char password_2[50]; // Оголошення масиву, який буде слугувати перевіркою паролю, коли користувач створює новий пароль

    fflush(stdin);  // Очищення вхідного буфера

    printf("Для початку, зареєструйте акаунт");
    printf("\nВкажіть нікнейм: ");
    fgets(myAccount->name, sizeof(myAccount->name), stdin);
    myAccount->name[strcspn(myAccount->name, "\n")] = '\0'; // Видалення символу нового рядка

    printf("\nВкажіть електронну пошту користувача: ");
    fgets(myAccount->email, sizeof(myAccount->email), stdin);
    myAccount->email[strcspn(myAccount->email, "\n")] = '\0'; // Видалення символу нового рядка
printf("\nВкажіть пароль: ");
    fgets(myAccount->password, sizeof(myAccount->password), stdin);
    myAccount->password[strcspn(myAccount->password, "\n")] = '\0'; // Видалення символу нового рядка

    printf("\nПовторіть пароль: ");
    fgets(password_2, sizeof(password_2), stdin);
    password_2[strcspn(password_2, "\n")] = '\0'; // Видалення символу нового рядка

    do {
        if (strcmp(myAccount->password, password_2) == 0) { // Перевірка пароля за допомогою функції strcmp()
            printf("Аккаунт зареєстровано\n");
        } else {
            printf("\nПаролі не збігаються\n");
            printf("\nВкажіть пароль: ");
            fgets(myAccount->password, sizeof(myAccount->password), stdin);
            myAccount->password[strcspn(myAccount->password, "\n")] = '\0'; // Видалення символу нового рядка

            printf("\nПовторіть пароль: ");
            fgets(password_2, sizeof(password_2), stdin);
            password_2[strcspn(password_2, "\n")] = '\0'; // Видалення символу нового рядка
        }
    } while (strcmp(myAccount->password, password_2) != 0);
}

void Output_of_account_data() // Функція виведення даних про аккаунт
{
    if (myAccount != NULL){
        printf("\nНікнейм: %s\n", myAccount->name);
        printf("Електронна пошта: %s\n", myAccount->email);
        printf("Пароль: %s\n", myAccount->password);
    }else{
        printf("\nАкаунт не зареєстровано\n");
    }
}

void List_of_music() // Оголошення функції, яка виводить список музики користувачеві
{
    printf("\nНазва та автор пісні                               Довжина пісні\n\n");
    for (int i = 0; i < size; i++) {
        int minutes = songs[i].time / 60;
        int seconds = songs[i].time % 60;
        printf("%-50s %d:%02d\n", songs[i].musicname, minutes, seconds);
    }
}

void Music_search() // Оголошення функції пошуку музики
{
    char target[50];

    printf("\nВведіть назву пісні або ім'я автора: ");
    scanf(" %[^\n]", target);

    bool found = false;

    for (int i = 0; i < size; i++) {
        if (strstr(songs[i].musicname, target) != NULL) {
            int minutes = songs[i].time / 60;
            int seconds = songs[i].time % 60;
            printf("\nНазва пісні: %s\n", songs[i].musicname);
            printf("Тривалість: %d:%02d\n", minutes, seconds);
            found = true;
        }
    }

    if (!found) {
        printf("\nПісні з назвою, що містить '%s', не знайдено\n", target);
    }
}

void Account_deletion() // Оголошення функції видалення акаунта
{
    int choice = -1;
    char password_3[50]=" ";
    printf("Для видалення акаунта, вкажіть пароль: ");
    scanf("%s", password_3);
    do{
        if (strcmp(myAccount->password, password_3) == 0) {
            printf("\nВи впевнені, що хочете видалити аккаунт? Ця дія є безповоротною! Так - 1 / Ні - 2");

            bool check = true;
            bool flag = true;

            do {
                do {
                    printf("\n\nВиберіть опцію: ");

                    // Перевірка на коректне введення
                    if (scanf("%d", &choice) != 1) {
                        printf("\nПомилкове введення. Спробуйте знову\n");

                        // Очищення буферу вводу
                        while (getchar() != '\n')
                            ;

                        // Продовження циклу з нового кроку
                        continue;
                    } else {
                        check = false;
                    }

                    if (choice != 1 && choice != 2) {
                        flag = true;
                        printf("\nПомилкове введення. Спробуйте знову\n");
                    } else {
                        flag = false;
                    }

                } while (check);
            } while (flag);

            // Очищення кожного поля окремо
            switch (choice) {
                case 1:
                    memset(myAccount->name, 0, 50);
                    memset(myAccount->email, 0, 50);
                    memset(myAccount->password, 0, 50);
                    free(myAccount); // Очищення пам'яті
                    myAccount = NULL; // Робимо вказівник недійсним
                    printf("\nВаші дані були остаточно видалені!\n");
                    Menu();
                    break;
                case 2:
                    printf("\nВи будете направлені у головне меню\n");
                    break;
                default:
                    break;
            }
            password_3[strcspn(password_3, "\n")] = '\0';
        } else{
            password_3[strcspn(password_3, "\n")] = '\0';
            printf("Повторіть спробу пізніше");
            }
    }while (strcmp(myAccount->password, password_3) != 0);
    }

void changeAccount()
{
    int choice = -1;
    char passwordcheck[50];

    printf("\nВведіть пароль для підтвердження особи: ");
    scanf("%s", passwordcheck);

    if (strcmp(myAccount->password, passwordcheck) == 0) { // Перевірка пароля за допомогою strcmp()
    do {
        printf("\nВиберіть, що саме ви хочете змінити:\n");
        printf("1) Нікнейм\n");
        printf("2) Ел. пошту\n");
        printf("3) Пароль\n");
        printf("4) Повернутися в меню\n");

        bool check = true;

        do {
            printf("\nВиберіть опцію: ");

            // Перевірка на коректне введення
            if (scanf("%d", &choice) != 1) {
                printf("\nПомилкове введення. Спробуйте знову\n");

                // Очищення буферу вводу
                while (getchar() != '\n');

                // Продовження циклу з нового кроку
                continue;
            } else {
                check = false;
            }

        } while (check);

        switch (choice) {
            case 1:
                Change_nickname();
                break;
            case 2:
                Change_email();
                break;
            case 3:
                Change_password();
                break;
            case 4:
                printf("\nПрограма завершена\n");
                break;
            default:
                printf("Невірний вибір. Спробуйте ще раз\n");
                break;
        }
    } while (choice != 4);
}
else{
    printf("\nПароль введено неправильно\n");
    printf("\nПовертаємось в головне меню\n");
}
}

void Change_nickname()
{
        char newUsername[50];
        printf("\nВведіть новий нікнейм: ");
        scanf(" %[^\n]", newUsername);
        while (getchar() != '\n');  // Очищення буфера вводу

        strcpy(myAccount->name, newUsername);
        printf("\nНікнейм користувача змінено успішно.\n");
}

void Change_password()
{
        char newPassword[50];
        char newPassword2[50];
        printf("\nВведіть новий пароль: ");
        scanf(" %[^\n]", newPassword);
        while (getchar() != '\n');  // Очищення буфера вводу
        printf("\nВведіть новий пароль ще раз:");
        scanf(" %[^\n]", newPassword2);
    do {
        if (strcmp(newPassword, newPassword2) == 0) { // Перевірка пароля за допомогою функції strcmp()
            strcpy(myAccount->password, newPassword);
            printf("Пароль успішно змінено");
        } else {
            printf("\nПаролі не збігаються\n");
            printf("\nВкажіть пароль: ");
            scanf("%s", newPassword);

            printf("\nПовторіть пароль: ");
            scanf("%s", newPassword2);
        }
    } while (strcmp(myAccount->password, newPassword) != 0);
}

void Change_email()
{
        char newEmail[50];
        printf("\nВведіть нову електронну пошту: ");
        scanf(" %[^\n]", newEmail);
        while (getchar() != '\n');  // Очищення буфера вводу

        strcpy(myAccount->email, newEmail);
        printf("\nЕлектронну пошту користувача змінено успішно.\n");
}

