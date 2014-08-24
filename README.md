bottle
======
+package com.fuzionsoftware.appstats;
2	+
3	+import java.util.ArrayList;
4	+import java.util.HashMap;
5	+
6	+import android.app.Activity;
7	+import android.content.pm.ApplicationInfo;
8	+import android.content.pm.PackageInfo;
9	+import android.graphics.drawable.Drawable;
10	+import android.view.Gravity;
11	+import android.view.View;
12	+import android.view.ViewGroup;
13	+import android.widget.AbsListView;
14	+import android.widget.BaseExpandableListAdapter;
15	+import android.widget.ImageView;
16	+import android.widget.TextView;
17	+
18	+public class PermissionAppExpandableAdapter extends BaseExpandableListAdapter {
19	+    
20	+	HashMap<String,ArrayList<PackageInfo>> permissionAppMap;	
21	+	Object[] mKeys;
22	+	Activity mCTX;
23	+	
24	+	PermissionAppExpandableAdapter( HashMap<String,ArrayList<PackageInfo>> appPermissionMap, Activity context)
25	+	{
26	+		permissionAppMap = appPermissionMap;
27	+		
28	+		mKeys = permissionAppMap.keySet().toArray();
29	+		mCTX = context;
30	+	}
31	+	
32	+    public Object getChild(int groupPosition, int childPosition) {
33	+        ApplicationInfo appInfo =  permissionAppMap.get(mKeys[groupPosition]).get(childPosition).applicationInfo;
34	+        
35	+        return mCTX.getPackageManager().getApplicationLabel(appInfo);
36	+    }
37	+
38	+    public long getChildId(int groupPosition, int childPosition) {
39	+        return childPosition;
40	+    }
41	+
42	+    public int getChildrenCount(int groupPosition) {
43	+        return permissionAppMap.get(mKeys[groupPosition]).size();
44	+    }
45	+
46	+    public TextView getGenericView() {
47	+        // Layout parameters for the ExpandableListView
48	+        AbsListView.LayoutParams lp = new AbsListView.LayoutParams(
49	+                ViewGroup.LayoutParams.MATCH_PARENT, 64);
50	+
51	+        TextView textView = new TextView(mCTX);
52	+        textView.setLayoutParams(lp);
53	+        // Center the text vertically
54	+        textView.setGravity(Gravity.CENTER_VERTICAL | Gravity.LEFT);
55	+        // Set the text starting position
56	+        textView.setPadding(36, 0, 0, 0);
57	+        return textView;
58	+    }
59	+
60	+    public View getChildView(int groupPosition, int childPosition, boolean isLastChild,
61	+            View convertView, ViewGroup parent) {
62	+    	Drawable img = permissionAppMap.get(mKeys[groupPosition]).get(childPosition).applicationInfo.loadIcon(mCTX.getPackageManager());
63	+    	
64	+    	View view = mCTX.getLayoutInflater().inflate(R.layout.app_layout, null);
65	+    	TextView textview = (TextView) view.findViewById(R.id.android_info_text);
66	+    	ImageView imageview = (ImageView) view.findViewById(R.id.android_info_image);
67	+    	
68	+    	textview.setText(getChild(groupPosition, childPosition).toString());    
69	+    	imageview.setImageDrawable(img);
70	+        return view;
71	+    }
72	+
73	+    public Object getGroup(int groupPosition) {
74	+    	String s = (String)mKeys[groupPosition];
75	+        return s.replaceAll("android.permission.", "");
76	+    }
77	+
78	+    public int getGroupCount() {
79	+        return mKeys.length;
80	+    }
81	+
82	+    public long getGroupId(int groupPosition) {
83	+        return groupPosition;
84	+    }
85	+
86	+    public View getGroupView(int groupPosition, boolean isExpanded, View convertView,
87	+            ViewGroup parent) {
88	+        TextView textView = getGenericView();
89	+        textView.setText(getGroup(groupPosition).toString());
90	+        return textView;
91	+    }
92	+
93	+    public boolean isChildSelectable(int groupPosition, int childPosition) {
94	+        return true;
95	+    }
96	+
97	+    public boolean hasStableIds() {
98	+        return true;
99	+    }
