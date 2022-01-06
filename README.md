# githubTest
 package com.example.ict2022;

import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.recyclerview.widget.LinearLayoutManager;
import androidx.recyclerview.widget.RecyclerView;

import android.content.Context;
import android.os.Bundle;

import com.google.firebase.database.DataSnapshot;
import com.google.firebase.database.DatabaseError;
import com.google.firebase.database.DatabaseReference;
import com.google.firebase.database.FirebaseDatabase;
import com.google.firebase.database.Query;
import com.google.firebase.database.ValueEventListener;

import java.util.ArrayList;

public class MainActivity extends AppCompatActivity {
   RecyclerView recyclerView;
   private DatabaseReference mRef;
   private ArrayList<Model> modelArrayList;
   private Context mContext;

   private RecyclerAdapter recyclerAdapter;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        recyclerView  = findViewById(R.id.recycler);
        LinearLayoutManager linearLayoutManager = new LinearLayoutManager(this);
        recyclerView.setLayoutManager(linearLayoutManager);
        recyclerView.setHasFixedSize(true);

        // firebase

        mRef = FirebaseDatabase.getInstance().getReference();

        //ArrayList

        modelArrayList = new ArrayList<>();

        //get data


        clearAll();
        GetDataFromFirebase();







    }

    private void GetDataFromFirebase() {

        Query query  = mRef.child("message");
        query.addValueEventListener(new ValueEventListener() {
            @Override
            public void onDataChange(@NonNull DataSnapshot dataSnapshot) {

                clearAll();

                for(DataSnapshot snapshot : dataSnapshot.getChildren()) {
                  Model model = new Model();
                  model.setImageUrl(snapshot.child("image").getValue().toString());
                  model.setName(snapshot.child("name").getValue().toString());
                  modelArrayList.add(model);
                }
                recyclerAdapter = new RecyclerAdapter(getApplicationContext(),modelArrayList);
                recyclerView.setAdapter(recyclerAdapter);
                recyclerAdapter.notifyDataSetChanged();
            }

            @Override
            public void onCancelled(@NonNull DatabaseError error) {

            }
        });



    }


    private  void clearAll() {
        if (modelArrayList !=null) {
            modelArrayList.clear();

            if (recyclerAdapter !=null) {
                recyclerAdapter.notifyDataSetChanged();
            }
        }
    }
}