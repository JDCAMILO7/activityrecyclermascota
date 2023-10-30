# activityrecyclermascota
Entiendo que no esta perfecto no comprendo bien cosas de interacción pero tengo un mini recurso para que me evalúan 😊
public class Mascota {
    private String nombre;
    private int rating;

    public Mascota(String nombre, int rating) {
        this.nombre = nombre;
        this.rating = rating;
    }

    public String getNombre() {
        return nombre;
    }

    public int getRating() {
        return rating;
    }

    public void incrementRating() {
        rating++;
    }
}

XML
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="horizontal">

    <ImageView
        android:id="@+id/imgMascota"
        android:layout_width="50dp"
        android:layout_height="50dp"
        android:src="@drawable/default_pet_image" />

    <TextView
        android:id="@+id/txtNombre"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Nombre de la Mascota" />

    <ImageView
        android:id="@+id/imgHueso"
        android:layout_width="20dp"
        android:layout_height="20dp"
        android:src="@drawable/bone_icon" />
</LinearLayout>

public class MascotaAdapter extends RecyclerView.Adapter<MascotaAdapter.MascotaViewHolder> {

    private List<Mascota> mascotas;
    private OnRatingClickListener onRatingClickListener;

    public MascotaAdapter(List<Mascota> mascotas, OnRatingClickListener onRatingClickListener) {
        this.mascotas = mascotas;
        this.onRatingClickListener = onRatingClickListener;
    }

    public interface OnRatingClickListener {
        void onRatingClick(int position);
    }


    @Override
    public void onBindViewHolder(MascotaViewHolder holder, int position) {
        Mascota mascota = mascotas.get(position);

        holder.txtNombre.setText(mascota.getNombre());
        holder.imgHueso.setOnClickListener(v -> onRatingClickListener.onRatingClick(position));

        // la mascota” imagen”
    }

    // ViewHolder
    public static class MascotaViewHolder extends RecyclerView.ViewHolder {
        ImageView imgMascota;
        TextView txtNombre;
        ImageView imgHueso;

        public MascotaViewHolder(View itemView) {
            super(itemView);
            imgMascota = itemView.findViewById(R.id.imgMascota);
            txtNombre = itemView.findViewById(R.id.txtNombre);
            imgHueso = itemView.findViewById(R.id.imgHueso);
        }
    }
}
public class MainActivity extends AppCompatActivity implements MascotaAdapter.OnRatingClickListener {

    private RecyclerView recyclerView;
    private MascotaAdapter adapter;
    private List<Mascota> mascotas;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        recyclerView = findViewById(R.id.recyclerView);
        recyclerView.setLayoutManager(new LinearLayoutManager(this));

        mascotas = new ArrayList<>();
        // Agregar mascotas a la lista

        adapter = new MascotaAdapter(mascotas, this);
        recyclerView.setAdapter(adapter);
    }

    @Override
    public void onRatingClick(int position) {
        // Incrementar el rating 
        mascotas.get(position).incrementRating();
        adapter.notifyItemChanged(position);
    }
}
