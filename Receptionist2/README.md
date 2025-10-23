# Receptionist2 API Testing Guide

## 🚀 Quick Setup

### 1. Create Receptionist2 Staff Member
Use the admin panel or staff registration API to create a new staff member with:
- **Staff Type**: `receptionist2`
- **Email**: `receptionist2@hospital.com` (or any email)
- **Password**: Set a password
- **Other required fields**: Fill as needed

### 2. Get Authentication Token
```bash
POST {{baseUrl}}/staff/login
Content-Type: application/json

{
  "email": "receptionist2@hospital.com",
  "password": "your-password"
}
```

Copy the `token` from the response.

### 3. Update Environment Variables
In Bruno, go to Environments → Development and update:
```
receptionist2Token: paste-your-token-here
```

## 📋 Available Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/receptionist2/test-auth` | GET | Test authentication |
| `/receptionist2/dashboard` | GET | Main dashboard |
| `/receptionist2/dashboard/stats` | GET | Dashboard statistics |
| `/receptionist2/patients/optometrist-checked` | GET | Patients checked by optometrists |
| `/receptionist2/patients/examination/:id` | GET | Detailed examination data |
| `/receptionist2/patients/search` | GET | Search patients |
| `/receptionist2/queue/status` | GET | All queue status |
| `/receptionist2/queue/optometrist` | GET | Optometrist queue |

## 🧪 Testing Order

1. **Test Auth** - Verify authentication works
2. **Dashboard** - Check main dashboard access
3. **Dashboard Stats** - Verify statistics endpoint
4. **Optometrist Queue** - Check current queue
5. **Queue Status** - Check all queues
6. **Optometrist Checked Patients** - Check patient list
7. **Patient Search** - Test search functionality
8. **Patient Examination Details** - Test detailed view (need valid examination ID)

## 🔧 Query Parameters

### Optometrist Checked Patients
- `date`: Filter by date (YYYY-MM-DD)
- `status`: Filter by status (AWAITING_DOCTOR, WITH_DOCTOR, COMPLETED)
- `patientName`: Search by name
- `tokenNumber`: Search by token
- `phone`: Search by phone

### Patient Search
- `query`: Search term (required)
- `searchType`: Type of search - "name", "token", or "phone" (required)

## 🐛 Common Issues

### JWT Malformed Error
```json
{
  "error": "Invalid token format",
  "message": "Authentication token is malformed"
}
```
**Solution**: Check token format, ensure it has 3 parts separated by dots.

### Access Denied
```json
{
  "error": "Insufficient staff permissions",
  "required": ["receptionist2"],
  "current": "receptionist"
}
```
**Solution**: Ensure staff member has `staffType = "receptionist2"`.

### No Data Found
```json
{
  "data": {
    "patients": [],
    "statistics": { "totalPatients": 0 }
  }
}
```
**Solution**: 
- Create test patients
- Have optometrists examine patients
- Check date filters (default is today)

## 📊 Sample Data Setup

To get meaningful test data:

1. **Create Patients**: Use patient registration
2. **Check-in Patients**: Use receptionist check-in
3. **Optometrist Examinations**: Have optometrists examine patients
4. **Queue Transfers**: Move patients through queues

## 🔍 Debugging Tips

1. **Check Server Logs**: Look for detailed error messages
2. **Verify Database**: Ensure data exists in tables:
   - `patients`
   - `patient_visits`
   - `optometrist_examinations`
   - `patient_queue`
3. **Test Token**: Use the "Test Auth" endpoint first
4. **Check Permissions**: Verify staff type and status

## 📈 Expected Response Formats

All endpoints return:
```json
{
  "success": true|false,
  "message": "Description",
  "data": { ... }
}
```

Error responses:
```json
{
  "success": false,
  "message": "Error description",
  "error": "Detailed error message"
}
```