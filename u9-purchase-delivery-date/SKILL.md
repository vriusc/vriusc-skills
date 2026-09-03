---
name: u9-purchase-delivery-date
description: Update `采购交期(内部)` for an exact purchase-order line in U9C. Use for requests mentioning a U9采购订单号、行号和内部采购交期日期, including incomplete requests that must be checked before any edit.
metadata:
  targets: [codex]
---

# U9C Purchase Delivery Date

Update one identified purchase-order line in `https://u9.jibot.com.cn:8098/U9C/`.

## Inputs

- purchase-order number
- line number
- new internal purchase delivery date in `YYYY.MM.DD` format

Extract all three values from natural language, such as `采购订单号 PO...，行号 12，采购交期 1999.01.04`. If any value is missing or the date is invalid, state exactly what is missing and stop before editing. Treat an unusually old or future date as suspicious and reconfirm it before saving.

## Workflow

1. Reuse an authenticated browser session or request credentials at runtime. Credentials never enter skill files or chat output.
2. Select enterprise `7-14备份账套`; typing its name is insufficient. Wait for U9C to load organizations and verify `苏州玖物智能科技股份有限公司` is selected.
3. Open `标准采购-列表`. U9C may render this page inside an iframe; use a browser surface that can inspect expanded iframe content or open the iframe URL in the same authenticated session.
4. Query the exact order number, open it, and match the exact line number. Verify both identifiers in the detail page.
5. Read the existing `采购交期(内部)`, then change only that field.
6. Before saving, report the order number, line number, old date, new date, enterprise, and organization. Obtain any action-time confirmation required by the active browser policy.
7. Click `保存` once. Completion requires a visible success message or the reloaded saved value.

## Boundaries

- Screenshots are evidence only unless the user explicitly identifies their values as task inputs.
- One request changes one exact line unless every additional line is explicitly listed.
- Scope excludes submit, audit, unaudit, delete, and workflow-state changes.
- A read-only field, ambiguous match, validation error, or permission error ends the attempt with a factual report.
