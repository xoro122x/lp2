#include<iostream>
using namespace std;

void selection_sort(int arr[], int n){
    for(int i=0;i<n-1;i++){
        int min_idx=i;
        for(int j=i+1;j<n;j++){
            if(arr[min_idx]>arr[j]){
                min_idx=j;
            }
        }
        int temp=arr[min_idx];
        arr[min_idx]=arr[i];
        arr[i]=temp;
    }

    // print element
    for(int i=0;i<n;i++){
        cout<<arr[i]<<" ";
    }

}


int main(){

    int n;
    cout<<"Enter no of elements:";
    cin>>n;

    int arr[n];
    for(int i=0;i<n;i++){
        cout<<"Enter "<<i+1<<"th element: ";
        cin>>arr[i];
    }

    selection_sort(arr, n);


return 0;

}