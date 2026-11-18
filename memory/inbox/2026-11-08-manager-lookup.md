# Manager lookup changed in IPR-4126

2026-11-08 · dev-a

The old note says managers are found by full name. That's out of date since IPR-4126:

- `EmailService` finds the HR manager and the project manager by id: `IEmployeeLookup.GetByIdAsync(employee.HrManagerId / ManagerId)`. Only active employees resolve, so a manager who left resolves to nothing.
- `GetEmailsByFullNamesAsync` and `FindSingleEmailByName` are gone.
- Namesakes: the Employee API leaves the manager id empty when the name is ambiguous, so LeoHR no longer has its own "exactly one match" check.
- An unresolved role is dropped with a warning (`Email {TemplateId} for employee {EmployeeId} is sent without {Role}`). If no To recipient is left, the send fails with 400.

Why: with name matching, a renamed manager or a namesake made emails go out without them.
