#include <stdbool.h>
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int kR = 0, kC=0;
int nR = 0,nC = 0, iR = 0;
int oI = 0, lI = 0, mI = 0;
char keywords[100][1000], identifiers[100][1000], mathOperator[1000], logicalOperator[1000];
char others[1000], numbers[100][1000];


bool isDelimiter(char ch)
{
    if (ch == ' ' || ch == '+' || ch == '-' || ch == '*' ||
            ch == '/' || ch == ',' || ch == ';' || ch == '>' ||
            ch == '<' || ch == '=' || ch == '(' || ch == ')' ||
            ch == '[' || ch == ']' || ch == '{' || ch == '}')
        return (true);
    return (false);
}

bool isOthers(char ch)
{
    if (ch == ' ' || ch == '/' || ch == ',' || ch == ';' ||
            ch == '(' || ch == ')' || ch == '[' || ch == ']' ||
            ch == '{' || ch == '}')
        return (true);
    return (false);
}


bool isOperator(char ch)
{
    if (ch == '+' || ch == '-' || ch == '*' ||
            ch == '/' || ch == '>' || ch == '<' ||
            ch == '=')
        return (true);
    return (false);
}
bool isMathoperator(char ch)
{
    if (ch == '+' || ch == '-' || ch == '*' ||
            ch == '/')
        return (true);
    return (false);
}
bool isLogicaloperator(char ch)
{
    if (ch == '>' || ch == '<' ||
            ch == '=')
        return (true);
    return (false);
}
bool validIdentifier(char* str)
{
    if (str[0] == '0' || str[0] == '1' || str[0] == '2' ||
            str[0] == '3' || str[0] == '4' || str[0] == '5' ||
            str[0] == '6' || str[0] == '7' || str[0] == '8' ||
            str[0] == '9' || isDelimiter(str[0]) == true)
        return (false);
    return (true);
}

bool isKeyword(char* str)
{

    if (!strcmp(str, "if") || !strcmp(str, "else") ||
            !strcmp(str, "while") || !strcmp(str, "do") ||
            !strcmp(str, "break") ||
            !strcmp(str, "continue") || !strcmp(str, "int")
            || !strcmp(str, "double") || !strcmp(str, "float")
            || !strcmp(str, "return") || !strcmp(str, "char")
            || !strcmp(str, "case") || !strcmp(str, "char")
            || !strcmp(str, "sizeof") || !strcmp(str, "long")
            || !strcmp(str, "short") || !strcmp(str, "typedef")
            || !strcmp(str, "switch") || !strcmp(str, "unsigned")
            || !strcmp(str, "void") || !strcmp(str, "static")
            || !strcmp(str, "struct") || !strcmp(str, "goto"))
            return (true);
        else
            return (false);

}

bool isInteger(char* str)
{
    int i, len = strlen(str);

    if (len == 0)
        return (false);
    for (i = 0; i < len; i++)
    {
        if (str[i] != '0' && str[i] != '1' && str[i] != '2'
                && str[i] != '3' && str[i] != '4' && str[i] != '5'
                && str[i] != '6' && str[i] != '7' && str[i] != '8'
                && str[i] != '9' || (str[i] == '-' && i > 0))
            return (false);
    }
    return (true);
}

bool isRealNumber(char* str)
{
    int i, len = strlen(str);
    bool hasDecimal = false;

    if (len == 0)
        return (false);
    for (i = 0; i < len; i++)
    {
        if (str[i] != '0' && str[i] != '1' && str[i] != '2'
                && str[i] != '3' && str[i] != '4' && str[i] != '5'
                && str[i] != '6' && str[i] != '7' && str[i] != '8'
                && str[i] != '9' && str[i] != '.' ||
                (str[i] == '-' && i > 0))
            return (false);
        if (str[i] == '.')
            hasDecimal = true;
    }
    return (hasDecimal);
}


char* subString(char* str, int left, int right)
{
    int i;
    char* subStr = (char*)malloc(
                       sizeof(char) * (right - left + 2));

    for (i = left; i <= right; i++)
        subStr[i - left] = str[i];
    subStr[right - left + 1] = '\0';
    return (subStr);
}


bool isExists(char* ara, char ch)
{
    for(int i = 0; ara[i] != '\0'; i++)
    {
        if(ara[i] == ch)
        {
            return true;
        }
    }

    return false;
}


void parse(char* str)
{
    int left = 0, right = 0;
    int len = strlen(str);

    while (right <= len && left <= right)
    {

        if (isOthers(str[right]) == true)
        {
            others[oI++] = str[right];
        }

        if (isDelimiter(str[right]) == false)
        {
            right++;
        }


        if (isDelimiter(str[right]) == true && left == right)
        {
            if (isOperator(str[right]) == true)
            {
                if (isLogicaloperator(str[right])== true && isExists(logicalOperator,str[right]) == false)
                {
                    logicalOperator[lI++] = str[right];
                }

                if(isMathoperator(str[right]) == true && isExists(mathOperator,str[right]) == false)
                {
                    mathOperator[mI++] = str[right];
                }
            }

            right++;
            left = right;
        }
        else if (isDelimiter(str[right]) == true && left != right
                 || (right == len && left != right))
        {

            char* subStr = subString(str, left, right - 1);

            if (isKeyword(subStr) == true)
            {
                int i = 0;
                for(i = 0; subStr[i] != '\n'; i++)
                {
                    keywords[kR][i] = subStr[i];
                }
                keywords[kR][i] = '\0';
                kR++;
            }


            else if (isInteger(subStr) == true || isRealNumber(subStr) == true)
            {

                int i = 0;
                for(i = 0; subStr[i] != '\n'; i++)
                {
                    numbers[nR][i] = subStr[i];
                }
                keywords[nR][i] = '\0';
                nR++;
            }
            else if (validIdentifier(subStr) == true
                     && isDelimiter(str[right - 1]) == false)
            {
                int i = 0;

                int flag = 1;

                for(int i = 0; i < iR; i++)
                {
                    if(strcmp(subStr, identifiers[i]) == 0)
                    {
                        flag = 0;
                        break;
                    }
                }

                if(flag == 1)
                {
                    for(i = 0; subStr[i] != '\n'; i++)
                    {
                        identifiers[iR][i] = subStr[i];
                    }
                    identifiers[iR][i] = '\0';
                    iR++;
                }

            }


            left = right;
        }
    }
    return;
}

int main()
{
    char str[1000], ch;


    FILE* stream;

    if ((stream = fopen("com.txt", "r")) == NULL)
    {
        printf("Error! opening file");
        exit(1);
    }

    int i = 0;
    while((ch = fgetc(stream)) != EOF)
    {
        str[i++] = ch;
    }

    str[i] = '\0';
    parse(str);

    printf("Math___ %s\n",mathOperator);
    printf("Logical Operator___ %s\n",logicalOperator);


    printf("Identifiers: ");

    for(int i = 0; i < iR; i++)
    {
        printf("%s ", identifiers[i]);
    }


    printf("\nNumbers: ");

    for(int i = 0; i < nR; i++)
    {
        printf("%s ", numbers[i]);
    }



    printf("\nKeyWords: ");

    for(int i = 0; i < kR; i++)
    {
        printf("%s ", keywords[i]);
    }


    printf("\nMath Operator: ");

    for(int i = 0; i < mI; i++)
    {
        printf("%c ", mathOperator[i]);
    }

    printf("\nLogical Operator: ");
    for(int i = 0; i < lI; i++)
    {
        printf("%c ", logicalOperator[i]);
    }


    return (0);
}
