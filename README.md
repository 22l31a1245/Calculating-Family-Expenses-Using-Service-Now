# Calculating Family Expenses using ServiceNow

A comprehensive expense calculation system built on the ServiceNow platform that enables families to track, manage, and analyze daily expenses in a centralized location.

![Family Expenses System](3%20Family%20Expenses.png)

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation & Setup](#installation--setup)
- [Project Structure](#project-structure)
- [Configuration](#configuration)
- [Business Rules](#business-rules)
- [Testing](#testing)
- [Usage](#usage)
- [Benefits](#benefits)
- [Screenshots](#screenshots)
- [Contributing](#contributing)
- [License](#license)

## 🎯 Project Overview

The **Calculating Family Expenses using ServiceNow** project is designed to provide families with a robust, automated expense management system. Built on the ServiceNow platform, this solution leverages the platform's powerful features including Business Rules, Forms and Lists, and Data Management capabilities to create a seamless financial tracking experience.

### Category
ServiceNow Application Developer

### Skills Required
- Business Rules
- Forms and Lists
- Data Management
- ServiceNow Platform Development

## ✨ Features

- **Expense Categorization**: Organize expenses by type and category
- **Budget Setting and Monitoring**: Set and track family budgets
- **Real-time Tracking**: Monitor expenses as they occur
- **Daily Expense Aggregation**: Automatic rollup of daily expenses to family level
- **Automated Updates**: Business rules automatically update family expenses when daily expenses are added
- **Reporting and Analysis**: Comprehensive reporting capabilities for financial insights
- **User-friendly Interface**: Intuitive forms and related lists for easy data entry
- **Scalability**: Supports families with varying financial complexities

## 📋 Prerequisites

- ServiceNow instance (Personal Developer Instance or higher)
- Admin rights to create tables and configure applications
- Basic understanding of ServiceNow platform
- Knowledge of JavaScript for Business Rules

## 🚀 Installation & Setup

### Step 1: ServiceNow Instance Setup
1. Set up a ServiceNow instance
2. Create a new Update Set for project configuration
3. Name the Update Set: "Family Expenses Management System"

![Instance Creation](1%20Create%20new%20instance.png)

### Step 2: Create Family Expenses Table
1. Navigate to **System Definition > Tables**
2. Create a new table: `u_family_expenses`
3. Configure the following fields:
   - **Date** (Date field)
   - **Amount** (Currency/Decimal field)
   - **Expense Details** (String/Text field)
4. Configure Number field as Auto-number
5. Set up form layout for user-friendly input

![Family Expenses Table Creation](2%20Creation%20of%20Table%20-%20Family%20Expenses.png)

### Step 3: Create Daily Expenses Table
1. Create another table: `u_daily_expenses`
2. Configure the following fields:
   - **Date** (Date field)
   - **Expense** (Currency/Decimal field)
   - **Comments** (String field)
   - **Family Reference** (Reference field to u_family_expenses)
3. Configure Number field as Auto-number
4. Set up form layout

![Daily Expenses Table Creation](4%20Creation%20of%20Table%20-%20Daily%20Expenses.png)

### Step 4: Configure Relationships
1. Create relationship between Family Expenses and Daily Expenses tables
2. Configure Related List on Family Expenses to display linked Daily Expenses
3. Set up proper query conditions for the relationship

![Daily Expenses Relationship](8%20Daily%20Expenses%20Relationship.png)

## ⚙️ Configuration

### Number Maintenance
Configure auto-numbering for both tables to ensure unique identifiers:

![Number Maintenance](6%20Number%20Maintainance.png)

### Form Configuration
- Family Expenses form: Display Date, Amount, and Expense Details
- Daily Expenses form: Display Date, Expense, Comments, and Family Reference

## 🔧 Business Rules

### Family Expenses Update Rule

**Purpose**: Automatically update or create Family Expenses records when Daily Expenses are added.

**When**: After Insert/Update on Daily Expenses table

**Script**:
```javascript
function executeRule(current, previous /*null when async*/) {
    var FamilyExpenses = new GlideRecord('u_family_expenses');
    FamilyExpenses.addQuery('u_date', current.u_date);
    FamilyExpenses.query();
    
    if(FamilyExpenses.next()) {
        // Update existing family expense record
        FamilyExpenses.u_amount += current.u_expense;
        FamilyExpenses.u_expense_details += " > " + current.u_comments + ": Rs." + current.u_expense + "/-";
        FamilyExpenses.update();
    } else {
        // Create new family expense record
        var NewFamilyExpenses = new GlideRecord('u_family_expenses');
        NewFamilyExpenses.u_date = current.u_date;
        NewFamilyExpenses.u_amount = current.u_expense;
        NewFamilyExpenses.u_expense_details = " > " + current.u_comments + ": Rs." + current.u_expense + "/-";
        NewFamilyExpenses.insert();
    }
}
```

![Family Expenses Business Rule](7%20Family%20Expenses%20Business%20Rule.png)

### Relationship Refinement Rule

**Purpose**: Filter related Daily Expenses based on date matching.

**Script**:
```javascript
function refineQuery(current, parent) {
    current.addQuery('u_date', parent.u_date);
    current.query();
}
```

## 🧪 Testing

### Test Scenarios
1. **Daily Expense Addition**: 
   - Add daily expenses and verify automatic aggregation under Family Expenses
   - Verify that amounts are correctly summed up
   
2. **Related List Functionality**:
   - Check that related lists display correctly
   - Verify filtering works properly
   
3. **Auto-numbering**:
   - Validate that both tables generate unique numbers automatically
   
4. **Expense Details Update**:
   - Confirm that expense details and totals are updated correctly
   - Verify the concatenation of expense descriptions

### Sample Test Data
- Create daily expenses for the same date
- Verify they roll up to a single family expense record
- Add expenses for different dates and verify separate family expense records are created

## 📱 Usage

### Adding Daily Expenses
1. Navigate to **Daily Expenses** table
2. Click **New** to create a new record
3. Fill in the required fields:
   - Date
   - Expense amount
   - Comments describing the expense
4. Save the record

### Viewing Family Expenses
1. Navigate to **Family Expenses** table
2. View aggregated expenses by date
3. Use the related list to see detailed daily expenses
4. Review expense details for comprehensive breakdown

![Daily Expenses Interface](5%20Daily%20Expenses.png)

![Family Expenditure View](9%20Family%20Expenditure.png)

## 🎯 Benefits

- **Centralized Expense Tracking**: Single platform for all family expense management
- **Automated Aggregation**: Reduces manual effort in calculating daily totals
- **Easy Reporting**: Built-in ServiceNow reporting capabilities for financial analysis
- **Error Reduction**: Automated calculations minimize human errors
- **Financial Awareness**: Enhanced visibility into family spending patterns
- **Scalability**: Easily adaptable to different family sizes and complexity levels
- **Data Integrity**: ServiceNow platform ensures data consistency and security

## 📸 Screenshots

The project includes visual documentation of each step:

1. **Instance Creation**: Initial ServiceNow setup
2. **Table Creation**: Family and Daily Expenses table configuration
3. **Form Design**: User interface layouts
4. **Relationship Setup**: Table relationships and related lists
5. **Business Rules**: Automation logic implementation
6. **Testing Results**: Validation of functionality

## 🔄 Project Structure

```
Family Expenses System/
├── Tables/
│   ├── u_family_expenses
│   └── u_daily_expenses
├── Business Rules/
│   ├── Family Expenses Update Rule
│   └── Relationship Refinement Rule
├── Forms/
│   ├── Family Expenses Form
│   └── Daily Expenses Form
├── Related Lists/
│   └── Daily Expenses on Family Expenses
└── Documentation/
    ├── Setup Screenshots
    └── Configuration Guide
```

## 🤝 Contributing

1. Fork the project
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.

## 🙏 Acknowledgments

- ServiceNow Community for platform documentation
- ServiceNow Developer Program for resources and tools
- Family financial management best practices

## 📞 Support

For support and questions:
- Create an issue in this repository
- Contact the development team
- Refer to ServiceNow community forums

---

**Note**: This project demonstrates the power of ServiceNow as a customizable platform beyond traditional IT Service Management, showcasing its capabilities in financial management and family budgeting applications.