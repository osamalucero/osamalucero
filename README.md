#define _CRT_SECURE_NO_WARNINGS 
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Stock {
    char isin[14];
    char name[50];
    int amount;
    float price;
    float dividend;
    char date[11];
    char buyer[50];
};


void extractAndSortStockNames(const char* file_name) {
    struct Stock stocks[100] = { 0 }; 
    int count = 0;

    FILE* file;
    errno_t err = fopen_s(&file, file_name, "r");
    if (err != 0 || file == NULL) {
        printf("Datei konnte nicht geöffnet werden.\n");
        return;
    }

   
    while (fscanf_s(file, "%s %s %d %f %f %s %s",
        stocks[count].isin, sizeof(stocks[count].isin),
        stocks[count].name, sizeof(stocks[count].name),
        &stocks[count].amount,
        &stocks[count].price,
        &stocks[count].dividend,
        sizeof(stocks[count].date), stocks[count].date,
        sizeof(stocks[count].buyer), stocks[count].buyer) == 7) {

        count++;
    }

    fclose(file);

    
    printf("Gelesene Aktiennamen:\n");
    for (int i = 0; i < count; i++) {
        printf("%s\n", stocks[i].name);
    }

   
    for (int i = 0; i < count - 1; i++) {
        for (int j = 0; j < count - i - 1; j++) {
            if (strcmp(stocks[j].name, stocks[j + 1].name) > 0) {
               
                struct Stock temp = stocks[j];
                stocks[j] = stocks[j + 1];
                stocks[j + 1] = temp;
            }
        }
    }

  
    printf("\nSortierte Aktiennamen:\n");
    for (int i = 0; i < count; i++) {
        printf("%s\n", stocks[i].name);
    }
}

int main() {
    const char* file_name = ""C:\Users\PC\OneDrive\Desktop\AplAufg1\AplAufg1\stocks.txt""; 
    extractAndSortStockNames(file_name);
    return 0;
}
