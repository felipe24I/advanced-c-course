## Challenge bit fields
You need to write a C program that uses bit fields within a structure to store properties of a box: opacity, fill color, border visibility, border color, and border style. 
The program should demonstrate the initial settings and then modify them to new values, displaying both sets of properties.

<img width="805" height="322" alt="image" src="https://github.com/user-attachments/assets/752a8994-68b8-4091-8c5e-676b90eeaffb" />

### Example output
```text
Original box settings:
Box is opaque.
The fill color is yellow.
Border shown.
The border color is green.
The border style is dashed.

Modified box settings:
Box is transparent.
The fill color is white.
Border shown.
The border color is magenta.
The border style is solid.
```

## Solution
```c
#include <stdio.h>

// Define color enum
typedef enum
{
    COLOR_BLACK = 0,
    COLOR_RED,
    COLOR_GREEN,
    COLOR_YELLOW,
    COLOR_BLUE,
    COLOR_MAGENTA,
    COLOR_CYAN,
    COLOR_WHITE
} Color;

// Define box type enum
typedef enum
{
    BOX_TRANSPARENCY = 0,
    BOX_OPAQUE
} BoxType;

// Define border type enum
typedef enum
{
    BORDER_SHOWN = 0,
    BORDER_HIDDEN
} BorderType;

// Define border style enum
typedef enum
{
    BORDER_STYLE_NONE = 0,
    BORDER_STYLE_SOLID,
    BORDER_STYLE_DOTTED,
    BORDER_STYLE_DASHED
} BorderStyle;

typedef struct onscreen_box
{
    unsigned int box_type:1;      // 1 bit
    unsigned int fill_color:3;    // 3 bit
    unsigned int border_type:1;   // 1 bit
    unsigned int border_color:3;  // 3 bit
    unsigned int border_style:2;  // 2 bit
} box;

// Helper functions to get string representations
const char* get_color_name(Color color) {
    const char* names[] = {"Black", "Red", "Green", "Yellow",
                           "Blue", "Magenta", "Cyan", "White"};
    return names[color];
}

const char* get_box_type_name(BoxType type) {
    const char* names[] = {"Transparency", "Opaque"};
    return names[type];
}

const char* get_border_type_name(BorderType type) {
    const char* names[] = {"Shown", "Hidden"};
    return names[type];
}

const char* get_border_style_name(BorderStyle style) {
    const char* names[] = {"None", "Solid", "Dotted", "Dashed"};
    return names[style];
}

void display_values(box b);
void modify_values(box *b);

int main()
{
    // CORRECTED: Initialize in the order of struct definition
    // struct order: box_type, fill_color, border_type, border_color, border_style
    box b1 = {
        BOX_OPAQUE,           // box_type
        COLOR_YELLOW,         // fill_color
        BORDER_SHOWN,         // border_type
        COLOR_GREEN,          // border_color
        BORDER_STYLE_DASHED   // border_style
    };

    printf("\nOriginal Box settings\n");
    printf("-----------------------\n");
    display_values(b1);

    modify_values(&b1);

    printf("\nModified Box settings\n");
    printf("-----------------------\n");
    display_values(b1);

    return 0;
}

void display_values(box b)
{
    printf("box type:      %s\n", get_box_type_name(b.box_type));
    printf("fill color:    %s\n", get_color_name(b.fill_color));
    printf("border type:   %s\n", get_border_type_name(b.border_type));
    printf("border color:  %s\n", get_color_name(b.border_color));
    printf("border style:  %s\n", get_border_style_name(b.border_style));
}

void modify_values(box *b)
{
    int temp;

    printf("\n--- Modify Box Properties ---\n");
    printf("Box type (0=Transparency, 1=Opaque): ");
    scanf("%d", &temp);
    b->box_type = temp % 2;  // Ensure value fits in 1 bit

    printf("Fill color (0=Black,1=Red,2=Green,3=Yellow,4=Blue,5=Magenta,6=Cyan,7=White): ");
    scanf("%d", &temp);
    b->fill_color = temp % 8;  // Ensure value fits in 3 bits (0-7)

    printf("Border type (0=Shown, 1=Hidden): ");
    scanf("%d", &temp);
    b->border_type = temp % 2;  // Ensure value fits in 1 bit

    printf("Border color (0=Black,1=Red,2=Green,3=Yellow,4=Blue,5=Magenta,6=Cyan,7=White): ");
    scanf("%d", &temp);
    b->border_color = temp % 8;  // Ensure value fits in 3 bits (0-7)

    printf("Border style (0=None,1=Solid,2=Dotted,3=Dashed): ");
    scanf("%d", &temp);
    b->border_style = temp % 4;  // Ensure value fits in 2 bits (0-3)
}
```
