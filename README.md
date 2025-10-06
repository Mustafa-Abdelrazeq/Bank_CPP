// Bank Project.cpp : This file contains the 'main' function. Program execution begins and ends there.
//

#include"LibInput.h"
const string path = "file.txt";


struct BankInfo
{
	string BankAccount;
	string PinCode;
	string Name;
	string Phone;
	string Balance;
	bool flage = false;
};

void ShowManue()
{
	cout << "==============================" << endl;
	cout << "Main Manue Screen\n";
	cout << "=============================="<<endl;
	cout << "[1] Show Client List."<<endl;
	cout << "[2] Add New Client." << endl;
	cout << "[3] Delete Client." << endl;
	cout << "[4] Update Client Info." << endl;
	cout << "[5] Find Client." << endl;
	cout << "[6] TransAction." << endl;
	cout << "[7] Exit." << endl;
	//system("pause");
}

void BackMainManue() {
	system("pause");
	system("cls");
	ShowManue();
}
// Show Client Section

bool IsCharInSeparator(char Char, string Separator) {
	for (char& x : Separator) {
		if (Char == x) {
			return true;
		}
	}
	return false;
}

// Split String Line And Append It In Vector
vector<string> ConvertLineOFStringWithSepToVector(string sent, string Sep = "#//#") {
	vector<string>VecotOfStringAfterSplitting;
	string NewPhrase = "";
	for (int x = 0; x < sent.length(); x++) {
		bool Check = IsCharInSeparator(sent[x], Sep);
		if (!Check) {
			NewPhrase += sent[x];
		}

		else if (Check && NewPhrase != "") {
			VecotOfStringAfterSplitting.push_back(NewPhrase);
			NewPhrase = "";
		}
	}
	if (NewPhrase != "")
		VecotOfStringAfterSplitting.push_back(NewPhrase);

	return VecotOfStringAfterSplitting;
}
// Splitting The Vector To Struct Of Client By Indexs
// And Retuen Struct Of Values
BankInfo ConvertVectorToStruct(vector<string>&StringVector) {

	BankInfo ClientInfo;
	ClientInfo.BankAccount = StringVector[0];
	ClientInfo.PinCode = StringVector[1];
	ClientInfo.Name = StringVector[2];
	ClientInfo.Phone = StringVector[3];
	ClientInfo.Balance = StringVector[4];

	return ClientInfo;
}
//Read File And Upload it In The Vector Of Struct
vector<BankInfo> readFileAndAppenditInVector(string path) {

	vector<string>vString;
	vector<BankInfo>vStruct;
	BankInfo ClientInfo;
	fstream file;

	file.open(path, ios::in);
	if (file.is_open()) {
		string line ;
		while (getline(file, line)) {
			if (line != "") // Check If Line Not Empty
			{
				vString = ConvertLineOFStringWithSepToVector(line);
				ClientInfo = ConvertVectorToStruct(vString);
				vStruct.push_back(ClientInfo);
			}

		}
	}file.close();
	return vStruct;
}

void Header(vector<BankInfo>& vStructClients) {
	cout << "\n";
	cout <<setw(50)<< "We Have [" << vStructClients.size()<<"] Clients.\n";
	cout << "_______________________________________________________________________________________________________\n\n";
	cout << setw(6)<<left << " BankAccount ";
	cout << setw(6) << left << "| PinCode ";
	cout << setw(30) << left << "|\t\t\tName";
	cout << setw(10) << left << "|\tPhone\t";
	cout << setw(10) << left << "|  Balance";
	cout << "\n_______________________________________________________________________________________________________\n\n";

}
void ShowClients(vector<BankInfo>& vStructClients) {
	Header(vStructClients);
	for (BankInfo &client  : vStructClients) {
		cout << setw(13) << left << client.BankAccount
			<< "| " << setw(8) << left << client.PinCode
			<< "| " << setw(49) << left << client.Name
			<< "| " << setw(14) << left << client.Phone
			<< "| " << setw(9) << left << client.Balance;
		cout << "\n";

	}
	cout << "_______________________________________________________________________________________________________\n\n";

}
//-------------------------------------------------------------------------------------------------------
 
														// Add Client Section

string CheckBankAccountToGetNewID(string path ) {
	vector<BankInfo>vStructClients;
	vStructClients = readFileAndAppenditInVector(path);
	string UserInputBankAccount = LibInput::UserInput("Enter BankAccount : ");

	for (BankInfo& client : vStructClients) {

			while (UserInputBankAccount == client.BankAccount) {
				cout << "\nBankAccount is already extists\n";
				UserInputBankAccount = LibInput::UserInput("Enter BankAccount : ");
				
			}
	}
	return UserInputBankAccount;

}
vector<BankInfo> AppendNewClientToVstruct( vector<BankInfo>& vStructClients) {
	BankInfo client;
	client.BankAccount = CheckBankAccountToGetNewID(path);
	cout << "Enter Your PinCode : ";
	getline(cin >> ws, client.PinCode);
	cout << "Enter Your Name : ";
	getline(cin, client.Name);
	cout << "Enter Your Phone : ";
	cin >> client.Phone;
	cout << "Enter Your Balance : ";
	cin >> client.Balance;
	vStructClients.push_back(client);
	return 	 vStructClients;
}
string ConvertStructToString( BankInfo stClient , string Sep = "#//#") {
	string Client = "";
	
	Client = stClient.BankAccount + Sep +
		stClient.PinCode + Sep +
		stClient.Name + Sep +
		stClient.Phone + Sep +
		stClient.Balance;

	return Client;
}
void AddNewClient(string path, vector<BankInfo>& vStructClients) {

	vector<BankInfo>NewvStructClients = AppendNewClientToVstruct(vStructClients);
	fstream file;
	file.open(path, ios::out);
	if (file.is_open()) {
		for (BankInfo &client : NewvStructClients) {
			file << ConvertStructToString(client) << endl;
		}
		file.close();
	}
	cout << "\nThe Process is Sucsseced"<<endl;
}
void AddMoreClients(string path, vector<BankInfo>& vStructClients) {
	cout << "\n___________________________\n";
	cout << "\n\tAdd Client\n";
	cout << "___________________________\n";
	string user = "y";
	cout << "Do you Want Add Client [y/n] : ";
	getline(cin , user);

	while (user == "y" || user == "Y") {
		AddNewClient(path, vStructClients);
		cout << "\nDo you Want Add More Client [y/n] : ";
		cin.ignore(numeric_limits<streamsize>::max(), '\n'); // امسح الـ newline اللي باقي
		getline(cin, user);
		system("cls");
	}
	
}
//____________________________________________________________________________________
											// Delete Client
string FindClientByBankAccount(string path , vector <BankInfo> &vStructClients) {
	vStructClients = readFileAndAppenditInVector(path);
	string UserInputBankAccount = LibInput::UserInput("Enter BankAccount : ");
	for (BankInfo& Client : vStructClients) {

		if (UserInputBankAccount == Client.BankAccount) {
			cout << "\nBankAccount : " << Client.BankAccount << endl;
			cout << "PinCode : " << Client.PinCode << endl;
			cout << "Name : " << Client.Name << endl;
			cout << "Phone : " << Client.Phone << endl;
			cout << "Balance : " << Client.Balance << endl;
		
			return Client.BankAccount;
		}
	}
	return "";
}
void WriteFileByVstruct(string path , vector <BankInfo>& vStructClients) {
	fstream file;
	file.open(path, ios::out);
	if (file.is_open()) {
		for (BankInfo &client : vStructClients) {

			file << ConvertStructToString(client) << endl;
		}
		
		file.close();
	}
}
vector<BankInfo> RemoveClientOfVector(string path, vector <BankInfo>& vStructClients) {
	char Choice = 'y';
	vector<BankInfo> vAfterDeleted;
	string Id = FindClientByBankAccount(path, vStructClients);
	if (Id != "") {

		cout << "Are You Sure Want Delete  : [y/n] : ";
		cin >> Choice;
		if (tolower(Choice) == 'y') {
			for (BankInfo& client : vStructClients) {
				if (client.BankAccount != Id) {
					vAfterDeleted.push_back(client);
				}
			}
		
			cout << "\nThe Client is Deleted.\n" << endl;
			vStructClients = vAfterDeleted;
			return vStructClients;
		}
		else
		{
			return vStructClients;
		}
	}


	else {
		cout << "Wrong Bank Account \n";
		return vStructClients;
    }
}

void DeleteClient(string path, vector<BankInfo>& vStructClients) {
	cout << "\n___________________________\n";
	cout << "\n\tDelete Client\n";
	cout << "___________________________\n\n";
	
	vector<BankInfo> vS = RemoveClientOfVector(path, vStructClients);
	WriteFileByVstruct(path, vS);
}


											// Update Client


bool FindClient(string Input , vector<BankInfo>&VectorOfStructures) {
	
	for (int x = 0; x < VectorOfStructures.size(); x++) {
		if (Input == VectorOfStructures[x].BankAccount) {
			cout << "\n";
			cout << "\t" << "BankAccount : " << setw(9) << VectorOfStructures[x].BankAccount << endl;
			cout << "\t" << "PinCode : " << setw(4) << VectorOfStructures[x].PinCode << endl;
			cout << "\t" << "Name : " << setw(5) << VectorOfStructures[x].Name << endl;
			cout << "\t" << "Phone : " << setw(9) << VectorOfStructures[x].Phone << endl;
			cout << "\t" << "BankBalance : " << setw(9) << VectorOfStructures[x].Balance << endl;
			return true;
		}
	}
	cout << "\nYour ID not In Bank\n";
	return false;
}

vector<BankInfo> stUpdateClient( vector<BankInfo>& VectorOfStructures) {
	string Input = LibInput::UserInput("Enter ID : ");
	char Choise = 'y';
	if (FindClient(Input, VectorOfStructures)) {

		cout << "Are You Sure Want Update Info  : [y/n]: ";
		cin >> Choise;
		if (tolower(Choise) == 'y') {
			for (int x = 0; x < VectorOfStructures.size(); x++) {
				if (Input == VectorOfStructures[x].BankAccount) {
					cout << "Enter Your PinCode : ";
					getline(cin >> ws, VectorOfStructures[x].PinCode);
					cout << "Enter Your Name : ";
					getline(cin, VectorOfStructures[x].Name);
					cout << "Enter Your Phone : ";
					getline(cin, VectorOfStructures[x].Phone);
					cout << "Enter Your BankBalance : ";
					getline(cin, VectorOfStructures[x].Balance);
					cout << "your Process Is succsseded\n";
					return VectorOfStructures;
				}
			}
		}
		else
		{
			return VectorOfStructures;

		}
	}
	else
	{
		cout << "\nFalied Process\n";
		return VectorOfStructures;
	}
	

	
}

void UpdateClient(string path, vector <BankInfo>& VectorOfStructures) {
	cout << "\n___________________________\n";
	cout << "\n\tUpdate Client\n";
	cout << "___________________________\n\n";
	vector < BankInfo> vS = stUpdateClient(VectorOfStructures);
	WriteFileByVstruct(path, vS);													
}
														//Find Client

bool FindClient(vector<BankInfo>&VectorOfStructures) {
	string Input = LibInput::UserInput("Enter BankAccount : ");
	for (int x = 0; x < VectorOfStructures.size(); x++) {
		if (Input == VectorOfStructures[x].BankAccount) {
			Header(VectorOfStructures);
				cout << setw(13) << left << VectorOfStructures[x].BankAccount
					<< "| " << setw(8) << left << VectorOfStructures[x].PinCode
					<< "| " << setw(49) << left <<VectorOfStructures[x].Name
					<< "| " << setw(14) << left <<VectorOfStructures[x].Phone
					<< "| " << setw(9) << left << VectorOfStructures[x].Balance;
				cout << "\n";

			
			cout << "_______________________________________________________________________________________________________\n\n";
			return true;
		}
	}
	cout << "\nYour ID not In Bank\n";
	return false;
}


void FindClientFunc(vector<BankInfo>&VectorOfStructures) {
	cout << "\n___________________________\n";
	cout << "\n\tFind Client\n";
	cout << "___________________________\n\n";
	FindClient(VectorOfStructures);
	
}
//-----------------------------------------------------------------------------------------------
//								********[Transaction]*********
														
void  ShowTransAction() {
	cout << "==============================" << endl;
	cout << "TransAction\n";
	cout << "==============================" << endl;
	cout << "[1] Deposit." << endl;
	cout << "[2] WithDrow." << endl;
	cout << "[3] Total Balance." << endl;
	cout << "[4] Main Manue." << endl;
	//int Num = LibInput::GetValidatedInt("Enter Your Choice : ");

	
}


//								********Deposit********

BankInfo IsBankAccountInBank(vector<BankInfo>& VectorOfStructures)
{
	string ID = LibInput::UserInput("\nEnter BankAccount : ");
	while (true) {
		for (BankInfo& Client : VectorOfStructures) {
			if (ID == Client.BankAccount) {
				cout << "\nBankAccount : " << Client.BankAccount << endl;
				cout << "PinCode : " << Client.PinCode << endl;
				cout << "Name : " << Client.Name << endl;
				cout << "Phone : " << Client.Phone << endl;
				cout << "Balance : " << Client.Balance << endl;

				return  Client;
			}
		}
		ID = LibInput::UserInput("\nEnter BankAccount : ");
	}
}
void UpdateStructInfo(BankInfo Client , vector<BankInfo>& VectorOfStructures) {

	for (BankInfo& CurrentClient : VectorOfStructures) {
		if (CurrentClient.BankAccount == Client.BankAccount) {
			CurrentClient.BankAccount = Client.BankAccount;
			CurrentClient.PinCode = Client.PinCode;
			CurrentClient.Name = Client.Name;
			CurrentClient.Phone = Client.Phone;
			CurrentClient.Balance = Client.Balance;
		}
	}
}
void UpdateDeposit(vector<BankInfo>& VectorOfStructures) {
	BankInfo Client = IsBankAccountInBank(VectorOfStructures);
	cout << "[Your Balance Now : ["<< Client.Balance<<"]\n";
	string Amount = LibInput::UserInput("\n-----------\nEnter Amount : ");
	double total = stod(Client.Balance) + stod(Amount);
	Client.Balance = to_string(round(float(total)));
	cout << "\n[Your Balance After Deposit : [" << Client.Balance << "]\n";

	UpdateStructInfo(Client, VectorOfStructures);
}
void Deposit(vector<BankInfo>& VectorOfStructures) {
	cout << "\n___________________________\n";
	cout << "\n\tDeposit\n";
	cout << "___________________________\n\n";
	UpdateDeposit(VectorOfStructures);
	//vector < BankInfo> Vstruct =
	WriteFileByVstruct(path, VectorOfStructures);

}
//							********WithDrow********

void UpdateWithDrow(vector<BankInfo>& VectorOfStructures) {
	BankInfo Client = IsBankAccountInBank(VectorOfStructures);
	cout << "[Your Balance Now : [" << Client.Balance << "]\n";
	string Amount = LibInput::UserInput("\n-----------\nEnter Amount : ");
	if (stod(Amount) <= stod(Client.Balance)) {
		double total = stod(Client.Balance) + stod(Amount) * -1;
		Client.Balance = to_string(round(float(total)));
		cout << "\n[Your Balance After Deposit : [" << Client.Balance << "]\n";

		UpdateStructInfo(Client, VectorOfStructures);
	}
	else
	{
		cout << "[ Your Amount Now Bigger than you Balance [" << Client.Balance << "] \n";
	}

}
void WithDrow(vector<BankInfo>& VectorOfStructures) {
	cout << "\n___________________________\n";
	cout << "\n\tWithDrow\n";
	cout << "___________________________\n\n";
	UpdateWithDrow(VectorOfStructures);
	WriteFileByVstruct(path, VectorOfStructures);
}
//							*****TotalBalance********
void TotalBalance(vector<BankInfo>& VectorOfStructures) {
	float TotalBalanceOfAllClients = 0;
	for (BankInfo &client : VectorOfStructures) {
		TotalBalanceOfAllClients += stof(client.Balance);
	}
	cout << "Total Balance Of All Clients is : " << TotalBalanceOfAllClients;
}

void AllFunc(string path, vector<BankInfo>& vStructClients);
void TransAction(string path, vector<BankInfo>& VectorOfStructures) {
	ShowTransAction();
	int Num = LibInput::GetValidatedInt("Enter Your Choice : ");
	while (Num > 0 && Num < 5) {


		switch (Num) {
		case 1:
			Deposit(VectorOfStructures);
			system("pause");
			system("cls");
			break;
		case 2:
			WithDrow(VectorOfStructures);
			system("pause");
			system("cls");
			break;
		case 3:
			TotalBalance(VectorOfStructures);
			system("pause");
			system("cls");
			break;
		default:
			system("cls");
			AllFunc(path, VectorOfStructures);
			system("pause");
			system("cls");
			break;

		}
		ShowTransAction();
		Num = LibInput::GetValidatedInt("Enter Your Choice : ");
	}
	system("cls");
	AllFunc(path, VectorOfStructures);
	system("pause");
	system("cls");


}
//-----------------------------------------------------------------


void AllFunc( string path , vector<BankInfo>& vStructClients) {
	int Input ;
	do
	{
		
		ShowManue();
	
		Input = LibInput::GetValidatedInt("\nEnter Number 1-7 : ");
		system("cls");
		switch (Input) {
		case   1:
			ShowClients(vStructClients);
			system("pause");
			system("cls");
			break;
		case 2:
			AddMoreClients(path, vStructClients);
			system("pause");
			system("cls");
			break;
		case 3:
			DeleteClient(path, vStructClients);
			system("pause");
			system("cls");
			break;

		case 4:
			UpdateClient(path, vStructClients);
			system("pause");
			system("cls");
			break;

		case 5:
			FindClientFunc(vStructClients);
			system("pause");
			system("cls");
			break;
		case 6:
			TransAction(path, vStructClients);
			system("pause");
			system("cls");
			break;

		default:
			break;
			/*system("pause");
			system("cls");*/
		}
	} while (Input > 0 && Input < 7);
	//BackMainManue();
}



int main()
{
	vector<BankInfo> vStructClients = readFileAndAppenditInVector(path);
	//ShowClients(vStructClients);


	AllFunc(path , vStructClients);
									//Add Client
	//AddMoreClients(path , vStructClients);
	//ReadVector(vStructClients);
	
									// Delete Client
	//DeleteClient(path , vStructClients);

									//Update Client Info
	//UpdateClient(path, vStructClients);
								//Find Client
	//FindClientFunc(vStructClients);
	//AllFunc( path, vStructClients);
	return 0;
}

