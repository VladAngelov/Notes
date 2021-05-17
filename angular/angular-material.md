# Angular - Material

## Autocomplete
*The autocomplete is a normal text input enhanced by a panel of suggested options.*

### Simple autocomplete

#### Start
Creating the autocomplete panel and the options displayed inside it. 
Each option should be defined by a **mat-option** tag. 

```typescript
    <mat-autocomplete #auto="matAutocomplete">
        <mat-option *ngFor="let option of options" [value]="option">
            {{option}}
        </mat-option>
    </mat-autocomplete>
```

#### Next
Create the input and set the matAutocomplete input to refer to the template reference we assigned to the autocomplete.

*Note: It is possible to use template-driven forms instead, if you prefer. We use reactive forms in this example because it makes subscribing to changes in the input's value easy. For this example, be sure to import **ReactiveFormsModule** from **@angular/forms** into your NgModule.*

Link the text input to its panel - can do this by exporting the autocomplete panel instance into a local template variable (here we called it "auto"), and binding that variable to the input's matAutocomplete property.
```typescript
    <input type="text"
       placeholder="Pick one"
       aria-label="Number"
       matInput
       [formControl]="myControl"
       [matAutocomplete]="auto">
```

## Adding a custom filter

*https://material.angular.io/components/autocomplete/overview*
