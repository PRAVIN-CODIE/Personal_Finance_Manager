Entity Classes (JPA Models)
Now, let's define the data models for Expense, Income, and FinancialGoal. Each model represents a database entity.

Expense.java
java
Copy code
package com.finance.model;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class Expense {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String category;
    private double amount;
    private String description;
    private String date;

    // Getters and setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    
    public String getCategory() { return category; }
    public void setCategory(String category) { this.category = category; }

    public double getAmount() { return amount; }
    public void setAmount(double amount) { this.amount = amount; }

    public String getDescription() { return description; }
    public void setDescription(String description) { this.description = description; }

    public String getDate() { return date; }
    public void setDate(String date) { this.date = date; }
}
Income.java
java
Copy code
package com.finance.model;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class Income {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String source;
    private double amount;
    private String date;

    // Getters and setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    
    public String getSource() { return source; }
    public void setSource(String source) { this.source = source; }

    public double getAmount() { return amount; }
    public void setAmount(double amount) { this.amount = amount; }

    public String getDate() { return date; }
    public void setDate(String date) { this.date = date; }
}
FinancialGoal.java
java
Copy code
package com.finance.model;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class FinancialGoal {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String goalName;
    private double targetAmount;
    private double currentAmount;
    private String targetDate;

    // Getters and setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    
    public String getGoalName() { return goalName; }
    public void setGoalName(String goalName) { this.goalName = goalName; }

    public double getTargetAmount() { return targetAmount; }
    public void setTargetAmount(double targetAmount) { this.targetAmount = targetAmount; }

    public double getCurrentAmount() { return currentAmount; }
    public void setCurrentAmount(double currentAmount) { this.currentAmount = currentAmount; }

    public String getTargetDate() { return targetDate; }
    public void setTargetDate(String targetDate) { this.targetDate = targetDate; }
}
4. Repository Layer
Repositories interact with the database to perform CRUD operations. These interfaces extend JpaRepository.

ExpenseRepository.java
java
Copy code
package com.finance.repository;

import com.finance.model.Expense;
import org.springframework.data.jpa.repository.JpaRepository;

public interface ExpenseRepository extends JpaRepository<Expense, Long> {
}
IncomeRepository.java
java
Copy code
package com.finance.repository;

import com.finance.model.Income;
import org.springframework.data.jpa.repository.JpaRepository;

public interface IncomeRepository extends JpaRepository<Income, Long> {
}
FinancialGoalRepository.java
java
Copy code
package com.finance.repository;

import com.finance.model.FinancialGoal;
import org.springframework.data.jpa.repository.JpaRepository;

public interface FinancialGoalRepository extends JpaRepository<FinancialGoal, Long> {
}
5. Service Layer
The service layer implements the core business logic of the application. It contains methods for adding, updating, and retrieving expenses, incomes, and financial goals.

FinanceService.java
java
Copy code
package com.finance.service;

import com.finance.model.Expense;
import com.finance.model.Income;
import com.finance.model.FinancialGoal;
import com.finance.repository.ExpenseRepository;
import com.finance.repository.IncomeRepository;
import com.finance.repository.FinancialGoalRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class FinanceService {

    @Autowired
    private ExpenseRepository expenseRepository;
    
    @Autowired
    private IncomeRepository incomeRepository;
    
    @Autowired
    private FinancialGoalRepository financialGoalRepository;

    // Add Expense
    public void addExpense(Expense expense) {
        expenseRepository.save(expense);
    }

    // Add Income
    public void addIncome(Income income) {
        incomeRepository.save(income);
    }

    // Add Financial Goal
    public void addFinancialGoal(FinancialGoal financialGoal) {
        financialGoalRepository.save(financialGoal);
    }

    // Get All Expenses
    public List<Expense> getAllExpenses() {
        return expenseRepository.findAll();
    }

    // Get All Incomes
    public List<Income> getAllIncomes() {
        return incomeRepository.findAll();
    }

    // Get All Financial Goals
    public List<FinancialGoal> getAllFinancialGoals() {
        return financialGoalRepository.findAll();
    }
}
6. Controller Layer
The controller handles the HTTP requests. Here, we’ll add endpoints to display expenses, incomes, and financial goals.

FinanceController.java
java
Copy code
package com.finance.controller;

import com.finance.model.Expense;
import com.finance.model.Income;
import com.finance.model.FinancialGoal;
import com.finance.service.FinanceService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@Controller
public class FinanceController {

    @Autowired
    private FinanceService financeService;

    // Home page - Show expenses, incomes, and goals
    @GetMapping("/")
    public String home(Model model) {
        List<Expense> expenses = financeService.getAllExpenses();
        List<Income> incomes = financeService.getAllIncomes();
        List<FinancialGoal> goals = financeService.getAllFinancialGoals();

        model.addAttribute("expenses", expenses);
        model.addAttribute("incomes", incomes);
        model.addAttribute("goals", goals);

        return "index";
    }

    // Add Expense
    @PostMapping("/add-expense")
    public String addExpense(@ModelAttribute Expense expense) {
        financeService.addExpense(expense);
        return "redirect:/";
    }

    // Add Income
    @PostMapping("/add-income")
    public String addIncome(@ModelAttribute Income income) {
        financeService.addIncome(income);
        return "redirect:/";
    }

    // Add Financial Goal
    @PostMapping("/add-goal")
    public String addGoal(@ModelAttribute FinancialGoal goal) {
        financeService.addFinancialGoal(goal);
        return "redirect:/";
    }
}
