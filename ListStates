package com.uscapitals;
/*
 * Author: Teresa Fanchiang
 * Date: 7/24/13
 * Program: CapitalsOfTheUnitedStates is a learning
 * tool that enables a user to select a state from a
 * ListView and see its capital.
 */
import java.io.FileNotFoundException;
import java.io.InputStream;
import java.util.Scanner;

import android.app.Activity;
import android.content.ContentValues;
import android.content.res.Resources;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.os.Bundle;
import android.support.v4.widget.SimpleCursorAdapter;
import android.view.Menu;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ListView;
import android.widget.TextView;

public class ListStates extends Activity
{
    SimpleCursorAdapter sca;
    ListView lv;
    String states[];
    String capitals[];
    TextView tv;
    Cursor c;
    @Override
//*******************************onCreate()*******************************
    protected void onCreate(Bundle savedInstanceState)
    {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list_states);
        SQLiteDatabase db = openOrCreateDatabase("sandc.db",
                SQLiteDatabase.CREATE_IF_NECESSARY,null);
        String sqlcmd = "create table IF NOT EXISTS " + 
                        "sandc(_id integer " +
                        "primary Key autoincrement, state text," +
                        " capital text);";
        db.execSQL(sqlcmd);
        c = db.query("sandc",null, null, 
                null, null, null, null, null);
        ContentValues cv = new ContentValues();
        c.moveToFirst();
        if(c.getCount()==0)
        {
            states = new String[50];
            capitals = new String[50];
            try
            {
                parseStates(states, capitals);
            } catch (FileNotFoundException e)
            {
                System.err.println(e);
            }
            for(int i = 0; i < states.length; i++)
            {
                //Filling database
                cv.put("state", states[i]);
                cv.put("capital", capitals[i]);
                db.insert("sandc", null, cv);
            }
        }
        tv = (TextView)findViewById(R.id.text);
        tv.setText("Click a state to see its capital.");
        sca = new SimpleCursorAdapter(getApplicationContext(),
                android.R.layout.simple_list_item_single_choice,
                c, new String[]{"state"}, new int[]
                {android.R.id.text1}, 0);
        lv = (ListView)findViewById(R.id.list);
        lv.setAdapter(sca);
        lv.setChoiceMode(ListView.CHOICE_MODE_SINGLE);
        c.moveToFirst();
        if(c.isAfterLast()==false)
        {
            lv.setOnItemClickListener(new 
                    AdapterView.OnItemClickListener()
            {
                @Override
                public void onItemClick(AdapterView<?>
                            av, View v, int position, long id)
                {
                    c.moveToPosition(position);
                    tv.setText("The capital of " + 
                    c.getString(c.getColumnIndex("state")) + " is " + 
                    c.getString(c.getColumnIndex("capital")) + ".");
                }       
            });         
        }
    }
//*****************************parseStates()******************************
    private void parseStates(String states[], String capitals[]) 
                             throws FileNotFoundException
    {
       Resources resources = getApplicationContext().getResources();
       InputStream is = resources.openRawResource(R.raw.us_states);
       Scanner sc = new Scanner(is);
       String line;
       int i = 0;
       sc.nextLine(); sc.nextLine();
       while(sc.hasNext())
       {
          line = sc.nextLine();
          String temp[] = line.split("\\s\\s+");
          if(temp.length >= 2)
          {
             if(temp.length == 2) 
             {
                states[i] = temp[0];
                capitals[i++] = temp[1];
             }
             else
             {
                states[i] = temp[0] + " " + temp[1];
                capitals[i++] = temp[2];
             }
          }
       }
    }
    @Override
//*************************onCreateOptionsMenu()**************************
    public boolean onCreateOptionsMenu(Menu menu)
    {
        getMenuInflater().inflate(R.menu.list_states, menu);
        return true;
    }
}
